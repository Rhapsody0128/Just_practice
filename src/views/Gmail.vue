<template>
  <div class="Gmail">
    <button @click="login()">登入</button>
    <button @click="logout()">登出</button>
    <button v-if="status==='登入中'" @click='sendEmail'>寄給我</button>
    {{status}}
  </div>
</template>
<<<<<<< HEAD
=======
<script src="https://apis.google.com/js/api.js"></script>
<script src="https://apis.google.com/js/client.js?onload=onClientLoad"></script>
>>>>>>> master
<script>

const DISCOVERY_DOCS = ['https://www.googleapis.com/discovery/v1/apis/gmail/v1/rest']
const SCOPES = 'https://www.googleapis.com/auth/gmail.send'
export default {
  data () {
    return {
      status: '未登入',
      name: '',
      googlemail: '',
      id: '',
      photoSrc: ''
    }
  },
  methods: {
    async login (event) {
      var name = ''
      var id = ''
      var googlemail = ''
      await window.gapi.auth2.getAuthInstance().signIn().then(function (GoogleUser) {
        name = GoogleUser.tt.Ad
        id = GoogleUser.tt.aU
        googlemail = GoogleUser.tt.bu
        console.log('登入成功')
      })
      this.status = '登入中'
      this.name = name
      this.id = id
      this.googlemail = googlemail
    },
    async logout () {
      await window.gapi.auth2.getAuthInstance().signOut().then(function () {
        console.log('登出成功')
      })
      this.status = '未登入'
    },
    sendEmail () {
      this.sendMessage(
        {
          From: this.googlemail,
          To: this.googlemail,
          Subject: 'Test'
        },
        'test'
      )
    },
    sendMessage (headers, message) {
      var email = ''

      for (var header in headers) {
        email += header += ': ' + headers[header] + '\r\n'
      }

      email += '\r\n' + message

      console.log(email)
      console.log(window.btoa(message).replace(/\+/g, '-').replace(/\//g, '_'))

      var sendRequest = window.gapi.client.gmail.users.messages.send({
        userId: 'me',
        resource: {
          raw: window.btoa(email).replace(/\+/g, '-').replace(/\//g, '_')
        }
      }).then(res => {
        console.log(res)
      })

      console.log(sendRequest)
    }
  },

  mounted () {
    window.gapi.load('client:auth2', function () {
      window.gapi.client.init({
        apiKey: process.env.VUE_APP_API_KEY,
        clientId: process.env.VUE_APP_CLIENT_ID,
        discoveryDocs: DISCOVERY_DOCS,
        scope: SCOPES
      })
    })
  }
}
</script>
