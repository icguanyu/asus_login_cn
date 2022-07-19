<template>
  <div>callback test</div>
</template>

<script>
// import Oidc from "oidc-client";
export default {
  name: "callback",
  mounted() {
    const vm = this;
    new window.Oidc.UserManager().signinRedirectCallback().then(function () {
      // window.location = process.env.VUE_APP_ASUS_LIVECHAT;
      console.log("/callback");
      vm.$router.push("/");
    });
  },
};
</script>
