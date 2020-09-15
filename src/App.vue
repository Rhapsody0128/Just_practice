<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/fb_login">FB login |</router-link>
      <router-link to="/google_login">GOOGLE login</router-link>
    </div>
    <router-view/>
  </div>
</template>

<script>

export default {
  mounted () {
    window.fbAsyncInit = function () {
      window.FB.init({
        appId: '2915384785415089',
        cookie: true,
        xfbml: true,
        version: 'v8.0'
      })
      window.FB.AppEvents.logPageView()
    }
  }

}
</script>
