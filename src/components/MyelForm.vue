<template>
    <form>
        <slot></slot>
    </form>
</template>

<script>
export default {
    name: 'MyelForm',
    provide() {
        return {
            myelForm: this
        };
    },
    props: {
        model: {
            type: Object,
            required: true
        },
        rules: {
            type: Object
        }
    },
    methods: {
        validate() {
            let all = [];
            console.log(this.$children);
            this.$children.forEach(field => {
                all.push(field.fieldValidator());
            });
            return Promise.all(all);
        }
    }
};
</script>

<style scoped></style>
