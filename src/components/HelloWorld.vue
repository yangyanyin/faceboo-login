<template>
  <div class="facebook-login">
    <h3>Facebook 登陆</h3>
    <a href="javascript:void(0);" @click.stop.prevent="facebookLogin">Sign In with Facebook`</a>
  </div>
</template>
<script>
export default {
  created () {
    (function (d, s, id) {
      // eslint-disable-next-line
      let js, fjs = d.getElementsByTagName(s)[0]
      if (d.getElementById(id)) {
        return
      }
      js = d.createElement(s)
      js.id = id
      js.src = 'https://connect.facebook.net/en_US/sdk.js'
      fjs.parentNode.insertBefore(js, fjs)
    }(document, 'script', 'facebook-jssdk'))

    // eslint-disable-next-line
    window.fbAsyncInit = function () {
      // eslint-disable-next-line
      FB.init({
        appId: 270202583150184,
        cookie: true,
        xfbml: true,
        version: 'v3.2' // use graph api version 3.2,
      })
    }
  },
  methods: {
    facebookLogin () {
      let _this = this
      // eslint-disable-next-line
      FB.getLoginStatus(function (response) {
        if (response.status === 'connected') {
          let userId = response.authResponse.userID
          _this.getFaceBookUserInfo(userId)
        } else {
          // eslint-disable-next-line
          FB.login(_this.checkLogin, { scope: 'email,public_profile', return_scopes: true })
        }
      })
    },
    checkLogin (response) {
      if (response.status === 'connected') {
        let userId = response.authResponse.userID
        this.getFaceBookUserInfo(userId)
      }
    },
    getFaceBookUserInfo (userId) {
      let _this = this
      // eslint-disable-next-line
      // fields=id,first_name,last_name,email 获取facebook对应的用户信息
      FB.api( '/' + userId + '/?fields=id,first_name,last_name,email', 'GET',{}, function (response) {
        _this.facebookSuccessLogin(response)
      })
    },
    //  facebook登陆成功后
    facebookSuccessLogin (response) {
      console.log(response)
    }
  }
}
</script>
