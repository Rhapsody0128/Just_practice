<template>
  <div class="Youtube">
    <input type="text" v-model="text">
    <button @click='search(text)'>搜尋</button>
    <br>
    <table class="table" v-if="musicCards" border="1">
      <th>
        <td>預覽圖</td>
        <td>標題</td>
        <td>ID</td>
        <td>描述</td>
        <td>連結</td>
      </th>
      <tr v-for="(musicCard, index) in musicCards" :key="index">
        <td class="pic"><img :src="musicCard.src" alt=""></td>
        <td><span>{{musicCard.title}}</span></td>
        <td><span>{{musicCard._id}}</span></td>
        <td><span>{{musicCard.description}}</span></td>
        <td><iframe width="100%" height="100%" :src="musicCard.url" frameborder="0"></iframe></td>
      </tr>
    </table>
  </div>
</template>
<script>

export default {
  data () {
    return {
      text: '',
      musicCards: ''
    }
  },
  methods: {
    search (text) {
      const request = window.gapi.client.youtube.search.list({
        part: 'snippet',
        q: text,
        maxResults: 24
      })
      var musicCards = []
      request.execute(function (response) {
        // 將結果把所需部分進行擷取
        response.items.map(item => {
          if (!item.id.playlistId) {
            var musicCard = {}
            musicCard._id = item.id.videoId
            musicCard.title = item.snippet.title
            musicCard.url = 'https://www.youtube.com/embed/' + musicCard._id
            musicCard.description = item.snippet.description
            musicCard.src = 'http://img.youtube.com/vi/' + musicCard._id + '/0.jpg'
            musicCards.push(musicCard)
          }
        })
      })
      this.musicCards = musicCards
    }
  },
  mounted () {
    window.gapi.client.load('youtube', 'v3', function () {
      window.gapi.client.setApiKey(process.env.VUE_APP_API_KEY)
    })
  }
}
</script>
<style lang="stylus">
.table{
  width 100%
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  th{
    width 100%
    display flex
    justify-content: space-between;
    padding 0
    td{
      width 100%
      display flex
      justify-content: center;
    }
  }
  tr{
    width 100%
    display flex
    justify-content: space-between;
    td{
      width 100%
    }
  }
  .pic{
    img{
      object-fit cover
      width 100%
      height 100%
    }
  }
}
</style>
