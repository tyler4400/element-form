<template>
    <div><input :value="value" @input="input" /></div>
</template>

<script>
export default {
    name: 'MyelInput',
    // todo 为什么这里我inject不进来
    // inject: ['myelForm', 'myelFormItem'],
    props: {
        value: {
            type: String,
            default: ''
        },
        type: {
            type: String,
            default: 'text'
        }
    },
    methods: {
        input($event) {
            console.log($event);
            this.$emit('input', $event.target.value);
            this.$parent.$emit('validator'); // 总觉得让parent来emit不是很好，万一parent不是myel-formItem呢？
        }
    }
};
</script>

<style scoped></style>
