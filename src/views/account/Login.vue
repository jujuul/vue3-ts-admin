/* eslint-disable @typescript-eslint/explicit-module-boundary-types */
<template>
  <div class="account">
    <div class="account__form">
      <a-form name="custom-validation" ref="formRef">
        <a-form-item>
          <label @click="set()">用户名</label>
          <a-input type="text" autocomplete="off" />
        </a-form-item>
        <a-form-item>
          <label>密码</label>
          <a-input type="password" autocomplete="off" />
        </a-form-item>
        <a-form-item>
          <div id="captcha-dom" class="nc-container"></div>
        </a-form-item>
        <a-form-item>
          <a-button type="primary" html-type="submit" block>登录</a-button>
        </a-form-item>
      </a-form>
      <div class="text-align fs-12 color-white">
        <a class="color-white" href="">忘记密码</a> |
        <a class="color-white" href="">注册</a>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { reactive, ref, toRefs } from "@vue/reactivity";
import "./captcha";
import { onMounted } from "@vue/runtime-core";
export default {
  name: "Login",
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  setup() {
    const count = ref(10);

    const formConfig = reactive({
      layout: {
        labelCol: { span: 4 },
        wrapperCol: { span: 14 },
      },
    });

    const data = toRefs(formConfig);

    const set = () => {
      count.value = 20;
    };

    onMounted(() => {
      var nc_token = [
        "FFFF0N00000000009931",
        new Date().getTime(),
        Math.random(),
      ].join(":"); // 没有做任何的响应
      var NC_Opt = {
        renderTo: "#captcha-dom",
        appkey: "FFFF0N00000000009931",
        scene: "nc_login",
        token: nc_token,
        customWidth: 285,
        trans: { key1: "code0" },
        elementID: ["usernameID"],
        is_Opt: 0,
        language: "cn",
        isEnabled: true,
        timeout: 3000,
        times: 5,
        apimap: {},
        callback: function (data) {
          window.console && console.log(nc_token);
          window.console && console.log(data.csessionid);
          window.console && console.log(data.sig);
        },
      };
      var nc = new noCaptcha(NC_Opt);
      nc.upLang("cn", {
        _startTEXT: "请按住滑块，拖动到最右边",
        _yesTEXT: "验证通过",
        _error300:
          '哎呀，出错了，点击<a href="javascript:__nc.reset()">刷新</a>再来一次',
        _errorNetwork:
          '网络不给力，请<a href="javascript:__nc.reset()">点击刷新</a>',
      });
    });
    return {
      ...data,
      set,
      count,
    };
  },
};
</script>

<style lang="scss">
@import "./style.scss";
</style>
