<template>
    <div id="app">
        <div>
            <h3>{{ formTitle }}</h3>
            <hr />
            <el-form :model="model" :rules="rules" ref="loginForm">
                <el-form-item label="用户名" prop="username">
                    <el-input
                        v-model="model.username"
                        autocomplete="off"
                    ></el-input>
                </el-form-item>
                <el-form-item label="确认密码" prop="password">
                    <el-input
                        type="password"
                        v-model="model.password"
                        autocomplete="off"
                    ></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="submitForm('loginForm')"
                        >提交</el-button
                    >
                </el-form-item>
            </el-form>
        </div>
        <hr />
        <div>
            <h1>test</h1>
            <label>v-model语法糖的原理</label>
            <myel-input
                :value="value"
                @input="value = arguments[0]"
            ></myel-input>
            <hr />

            <h1>仿写的el-form</h1>
            <myel-form :model="model" :rules="rules">
                <myel-form-item label="用户名" prop="username">
                    <myel-input
                        type="text"
                        v-model="model.username"
                    ></myel-input>
                </myel-form-item>
                <myel-form-item label="密码" prop="password">
                    <myel-input
                        type="password"
                        v-model="model.password"
                    ></myel-input>
                </myel-form-item>
            </myel-form>
        </div>
    </div>
</template>

<script>
import MyelInput from '@/components/MyelInput';
import MyelFormItem from '@/components/MyelFormItem';
import MyelForm from '@/components/MyelForm';

export default {
    name: 'app',
    components: {
        MyelInput,
        MyelFormItem,
        MyelForm
    },
    data() {
        return {
            formTitle: 'element表单',
            value: '',
            model: { username: 'tom', password: '' },
            rules: {
                username: [
                    { required: true, message: '请输入用户名' },
                    { min: 6, max: 10, message: '请输入6~10的用户名' }
                ],
                password: [{ required: true, message: '请输入密码' }]
            }
        };
    },
    methods: {
        submitForm(form) {
            this.$refs[form]
                .validator()
                .then(value => {
                    alert('请求登录!' + value);
                })
                .catch(err => {
                    alert('校验失败!' + err);
                });
        }
    }
};
</script>

<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
