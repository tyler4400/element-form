<template>
    <div><input :value="value" @input="input" /></div>
</template>

<script>
export default {
    name: 'MyelInput',
    props: ['value'],
    methods: {
        input($event) {
            console.log($event);
            this.$emit('input', $event.target.value);
        }
    }
};
</script>

<style scoped></style>
