<template>
  <div>
    <v-header :seller='seller'></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to='/goods'>商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to='/ratings'>评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to='/seller'>商家</router-link>
      </div>
    </div>
    <router-view></router-view>
  </div>
</template>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
<script>

import header from './components/header/header'
export default {
  name: 'App',
  components:{
    'v-header':header
  },
  data(){
    return{
      seller:{}
    }
  },
  created(){
    this.$http.get('/api/seller').then((res)=>{
      res=JSON.parse(res.request.response)
      if(res.code==0){
        console.log(res)
       this.seller=res.data
      }
    })
  }
}
</script>

<style lang="stylus">
    .tab
      display: flex
      width: 100%
      height: 40px
      line-height :40px
      .tab-item
        flex:1
        text-align:center
        &>a
          display:block
          font-size:14px
          color: rgb(77,85,93)
          &.active
            color:rgb(240,20,20)
</style>

