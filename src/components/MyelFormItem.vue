<template>
    <div>
        <label
            >{{ label }}
            <slot></slot>
            <p class="error-text" v-show="errStatus">{{ errMessage }}</p>
        </label>
    </div>
</template>

<script>
import AsyncValidator from 'async-validator';
export default {
    name: 'MyelFormItem',
    provide() {
        return {
            myelFormItem: this
        };
    },
    inject: ['myelForm'],
    props: ['label', 'prop'],
    data() {
        return {
            errMessage: '',
            errStatus: false
        };
    },
    methods: {
        fieldValidator() {
            let promise;
            const rule = this.myelForm.rules[this.prop];
            const value = this.myelForm.model[this.prop];

            //下面使用的async-validator库
            const descriptor = { [this.prop]: rule };
            const validator = new AsyncValidator(descriptor);
            promise = new Promise((resolve, reject) => {
                validator
                    /* 下面本想用promise ，但发现不支持， 可官方文档明明写的是返回一个promise；
                     *  https://www.npmjs.com/package/async-validator#validate
                     * */
                    // .validate({ [this.prop]: value })
                    // .then(() => {
                    //     console.log(arguments);
                    //     this.errMessage = '';
                    //     this.errStatus = false;
                    // })
                    // .catch(errors => {
                    //     console.log(arguments);
                    //     this.errMessage = errors[0].message;
                    //     this.errStatus = true;
                    // });
                    .validate({ [this.prop]: value }, errors => {
                        if (errors) {
                            this.errMessage = errors[0].message;
                            this.errStatus = true;
                            reject(errors);
                        } else {
                            resolve();
                            this.errMessage = '';
                            this.errStatus = '';
                        }
                    });
            });
            return promise;
        }
    },
    mounted() {
        this.$on('validator', () => {
            this.fieldValidator()
                .then(() => {
                    console.log('校验通过');
                })
                .catch(errors => {
                    console.log(errors);
                });
        });
    }
};
</script>

<style scoped>
.error-text {
    color: red;
}
</style>
