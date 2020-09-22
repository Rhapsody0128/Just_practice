<template>
  <div class="FB_login">
    <button @click="fblogin()">登入</button>
    <button @click="fblogout()">登出</button>
    <br>
    <div class="container">
      <img v-if="photoSrc" :src="photoSrc" alt="">
      <h1 v-if="name">姓名:{{name}}</h1>
      <h1 v-if="name">ID:{{userId}}</h1>
    </div>
  </div>
</template>
<script>

export default {
  data () {
    return {
      userId: '',
      photoSrc: '',
      name: ''
    }
  },
  methods: {
    fblogin () {
      window.FB.login(response => {
        window.FB.api('/me', response => {
          this.userId = response.id
          this.photoSrc = `http://graph.facebook.com/${response.id}/picture?type=large`
          this.name = response.name
          console.log(response)
        }, { scope: 'public_profile,email' })
      })
    },
    fblogout () {
      window.FB.logout(response => {
        this.userId = ''
        this.photoSrc = ''
        this.name = ''
      })
    }
  },
  mounted () {
    (function (d, s, id) {
      var js; var fjs = d.getElementsByTagName(s)[0]
      if (d.getElementById(id)) { return }
      js = d.createElement(s); js.id = id
      js.src = 'https://connect.facebook.net/en_US/sdk.js'
      fjs.parentNode.insertBefore(js, fjs)
      console.log(js)
    }(document, 'script', 'facebook-jssdk'))
    window.fbAsyncInit = function () {
      window.FB.init({
        appId: process.env.VUE_APP_FB_API,
        cookie: true,
        xfbml: true,
        version: 'v8.0'
      })
      window.FB.AppEvents.logPageView()
    }
  }
}
</script>
