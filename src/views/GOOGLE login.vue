<template>
  <div class="GOOGLE_login">
    <button @click="googlelogin()">登入</button>
    <button @click="googlelogout()">登出</button>
    <div class="container">
      <img v-if="photoSrc" :src="photoSrc" alt="">
      <h1 v-if="googlemail">信箱:{{googlemail}}</h1>
      <h1 v-if="name">姓名:{{name}}</h1>
      <h1 v-if="id">ID:{{id}}</h1>
    </div>
  </div>
</template>

<script>
const CLIENT_ID = '620173465-vtba7981i677t33lueo54hbca0pl0qke.apps.googleusercontent.com'
const DISCOVERY_DOCS = ['https://www.googleapis.com/discovery/v1/apis/people/v1/rest']

export default {
  data () {
    return {
      name: 'aa',
      googlemail: '',
      id: '',
      photoSrc: '',
      result: {}
    }
  },
  methods: {
    async googlelogin () {
      console.log(this.name)
      const auth2 = window.gapi.auth2.getAuthInstance()// 取得GoogleAuth物件
      var name = ''
      var id = ''
      var photoSrc = ''
      var googlemail = ''
      await auth2.signIn().then(function (GoogleUser) {
        console.log('Google登入成功')
        name = GoogleUser.tt.Ad
        id = GoogleUser.tt.aU
        googlemail = GoogleUser.tt.bu
        photoSrc = GoogleUser.tt.jK
        // const user_id = GoogleUser.getId()// 取得user id，不過要發送至Server端的話，為了資安請使用id_token，本人另一篇文章有範例：https://dotblogs.com.tw/shadow/2019/01/31/113026
        // console.log(`user_id:${user_id}`)
        // const AuthResponse = GoogleUser.getAuthResponse(true) // true會回傳包含access token ，false則不會
        // const id_token = AuthResponse.id_token// 取得id_token
        // people.get方法參考：https://developers.google.com/people/api/rest/v1/people/get
        window.gapi.client.people.people.get({
          resourceName: 'people/me',
          // 通常你會想要知道的用戶個資↓
          personFields: 'names,emailAddresses,photos'
        }).then(function (res) {
          // success
          // console.log(res.result)
        })
      },
      function (error) {
        console.log('Google登入失敗')
        console.log(error)
      })
      this.name = name
      this.id = id
      this.photoSrc = photoSrc
      this.googlemail = googlemail
    },
    googlelogout () {
      const auth2 = window.gapi.auth2.getAuthInstance() // 取得GoogleAuth物件
      auth2.disconnect().then(function () {
        console.log('User disconnect.')
      })
      this.name = ''
      this.id = ''
      this.photoSrc = ''
      this.googlemail = ''
    }

  },
  mounted () {
    window.gapi.load('client', function () {
      window.gapi.client.init({
        // client_id 和 scope 兩者參數必填
        clientId: CLIENT_ID,
        // scope列表參考：https://developers.google.com/people/api/rest/v1/people/get
        // "profile"是簡寫，要用完整scope名稱也可以
        scope: 'profile', // "https://www.googleapis.com/auth/userinfo.profile",
        discoveryDocs: DISCOVERY_DOCS
      })
    })
  }

}
</script>
