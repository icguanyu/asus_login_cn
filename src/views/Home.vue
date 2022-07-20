<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "Home",
  mounted() {
    this.login();
  },
  methods: {
    login() {
      const vm = this;
      const client_id = "ae1a4aafa7d73032";
      const authority = "https://acc-account.asus.com.cn";
      // const host = "https://livechat.asus.com.cn/chat/#/";

      const mgr = new window.Oidc.UserManager({
        userStore: new window.Oidc.WebStorageStateStore(),
        authority: authority,
        client_id: client_id,
        redirect_uri: "https://livechat.asus.com.cn/chat/callback.html",
        // post_logout_redirect_uri: redirect_uri,
        // silent_redirect_uri: redirect_uri,
        // accessTokenExpiringNotificationTime: 60,
        response_type: "id_token token",
        scope: "openid profile DefaultApi",
        filterProtocolClaims: true,
        // automaticSilentRenew: true,
        //loadUserInfo: true,
        metadata: {
          issuer: authority,
          authorization_endpoint: authority + "/connect/authorize",
          userinfo_endpoint: authority + "/connect/userinfo",
          end_session_endpoint: authority + "/connect/endsession",
          jwks_uri: authority + "/.well-known/openid-configuration/jwks",
        },
      });

      window.Oidc.Log.logger = console;

      console.log("account.login.status:", account.login.status);
      let isLogin = account.login.status;
      
      if (isLogin) {
        this.getUser(mgr, function (user) {
          if (user) {
            console.log("user:", user);
          } else {
            console.log("user null!!");
            this.signIn(mgr);
          }
        });
      }
      // mgr
      //   .getUser()
      //   .then(function (user) {
      //     console.log("user", user);
      //     if (user == null) {
      //       // alert("user null! start sign in.");
      //       vm.signIn(mgr);
      //     } else {
      //       console.log("getUser success:", user);
      //     }
      //   })
      //   .catch((err) => {
      //     console.log(err);
      //   });
    },
    getUser(mgr, callback) {
      mgr.getUser().then(callback);
    },
    signIn(mgr) {
      mgr.signinRedirect().catch(function (err) {
        console.log(err);
      });
    },
  },
};
</script>
