<template>
  <div class="container">
    <global-header :user="user"></global-header>
    <form action="">
      <div class="mb-3">
        <label class="form-label">邮箱地址</label>
        <validate-input :rules="emailRules" v-model="emailVal" placeholder="请输入邮箱地址" type="text" ></validate-input>
      </div>
      <div class="mb-3">
        <label class="form-label">密码</label>
        <validate-input  :rules="passwordRules" :min-length="6" placeholder="请输入密码" type="password" ></validate-input>
      </div>
      <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">密码</label>
        <input type="password" class="form-control" id="exampleInputPassword1">
      </div>
      <div class="mb-3 form-check">
        <input type="checkbox" class="form-check-input" id="exampleCheck1">
        <label class="form-check-label" for="exampleCheck1">Remember</label>
      </div>
      <button type="submit" class="btn btn-primary">登陆</button>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref } from 'vue';
import 'bootstrap/dist/css/bootstrap.min.css';
import ValidateInput, { RulesProp } from "@/hooks/ValidateInput.vue";
// import ColumnList, { ColumnProps } from './hooks/ColumnList.vue';
import GlobalHeader, { UserProps } from './components/GlobalHeader.vue';
import testData from './testData';

const currentUser: UserProps = {
  isLogin: true,
  id: 1,
  name: 'admin'
}

export default defineComponent({
  name: 'App',
  components: {
    // ColumnList,
    ValidateInput,
    GlobalHeader
  },
  inheritAttrs: false,
  setup() {
    const emailRules: RulesProp = [
      {
        type: 'required', message: '电子邮箱地址不能为空'
      },
      {
        type: 'email', message: '请输入正确的电子邮箱地址格式'
      },
    ]
    const passwordRules: RulesProp = [
      {
        type: 'required', message: '密码不能为空'
      },
      {
        type: 'minLength', message: ''
      },

    ]
    const emailVal = ref(null)
    return {
      list: testData,
      user: currentUser,
      emailRules,
      passwordRules,
      emailVal
    }
  }
});
</script>

<style>
</style>
