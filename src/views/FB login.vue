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
  }
}
</script>
