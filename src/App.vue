<template>
  <div id="app">
    <div id="nav">
      <router-link class="router" to="/">Home</router-link>
      <router-link class="router" to="/fb_login">FB login</router-link>
      <router-link class="router" to="/google_login">GOOGLE login</router-link>
      <router-link class="router" to="/youtube">Youtube</router-link>
      <router-link class="router" to="/gmail">Gmail</router-link>
    </div>
    <keep-alive >
      <router-view id='view' />
    </keep-alive>
  </div>
</template>

<script>

export default {
  mounted () {

  }

}
</script>
<style lang="stylus">
#nav{
  display flex
  justify-content space-around
  align-items center
  transition 1s
}
#view{
  text-align center
  margin-top 5%
}
.router{
  color rgb(70,50,30)
  font-size 2.5rem
  transition 0.5s
}
.router:hover{
  color wheat
}
#nav:hover{
  background rgb(120,120,120,0.2)
}
@media(max-width : 768px){
  #nav{
    display flex
    justify-content space-around
    align-items center
    transition 1s
    flex-direction column
  }
}
</style>
