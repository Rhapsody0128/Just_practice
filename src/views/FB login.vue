<template>
  <div class="FB_login">
    <button @click="fblogin()">登入</button>
    <!-- <button @click="logout()">登出</button> -->
  </div>
</template>
<script>

export default {
  data () {
    return {
    }
  },
  methods: {
    fblogin () {
      window.FB.login(response => {
        console.log(response)
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
    }(document, 'script', 'facebook-jssdk'))
  }
}
</script>
