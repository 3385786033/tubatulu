<template>
  <div class="searchGoods-zhb">
    <header class="searchGoods-head">
      <form action="">
        <input v-model="searchVal" placeholder="Devondale" class="searchGoods-input-zhb" @change="serchs">
        <i v-on:click="searchGoodsDelete"></i>
      </form>
    </header>
    <section class="searchGoods-list" v-if="type === 'A'">
      <ul>
        <li v-for="(itm,index) in Goodslist" :key="index">
          <a href="javascript:;">
            <img :src="itm.goods_image" alt="">
          </a>
          <div class="listGoods-descr-zhb">
            <p class="listGoods-mesg-zhb">{{ itm.goods_name }}</p>
            <p class="listGoods-pri-zhb">${{ itm.current_price }}</p>
          </div>
        </li>
      </ul>
      <footer class="searchGoods-footer-zhb">
        已结到底了~
      </footer>
    </section>

    <!--没有商品-->
    <div class="No_searchGoods-zhb" v-else-if="type === 'B'">
      <img src="../../assets/icon/none@2x.png" alt="">
      <p>没有收到相关商品~</p>
    </div>
    <!--加载失败-->
    <div class="lodErr-zhb" v-else-if="type === 'C'">
      <img src="../../assets/icon/failtoload@2x.png" alt="">
      <p>加载失败</p>
    </div>
    <!--服务器繁忙-->
    <div class="Servererr-zhb" v-else-if="type === 'D'">
      <img src="../../assets/icon/nodata@2x.png" alt="">
      <p>服务器繁忙</p>
      <input type="button" value="刷新" @click="refresh_btn">
    </div>

  </div>
</template>
<style>
  @import "searchGoods.css";
</style>
<script>
  export default {
    name: 'searchGoods',
    data () {
      return {
        type: 'B',
        searchVal: '',
        Goodslist: []
      }
    },
    methods: {
      searchGoodsDelete: function () {
        var searchText = document.querySelector('.searchGoods-input-zhb')
        searchText.value = ''
      },
      refresh_btn: function () {
        window.location.reload(true)
      },
      serchs: function () {
        if(this.serchVal == '') {
          return
        }else {
           this.$axios.get('/products?key_words='+this.searchVal).then((response)=>{
             let listAS = new getUrlprson(response,'goods_list')
             this.Goodslist = listAS.nows()
             if(this.Goodslist.length == 0){
               this.type = 'B'
             }else {
               this.type = 'A'
             }
           }).catch(function (err) {console.log(err)})
        }
      }
    },
    computed: {
      list: function () {
        let _this = this
        let arrGoods = []
        for ( let i = 0;i<arrGoods.length;i++) {

        }
        if (arrGoods.length == 0) {
          // this.type == 'B'
        }
      }
    },
    created () {

    }
  }
  function getUrlprson(dates,pars,bannerImg) {
    this.pars = pars
    this.bannerImg = bannerImg
    this.dates = dates.data.data[this.pars]
    this.showbanner = function () {
      let arrs = [];
      if (this.dates != null) {
        for (let i = 0; i < this.dates.length; i++) {
          arrs.push(this.dates[i][bannerImg])
        }
        return arrs
      }
    }
    this.navigation = function () {
      let naviges = []
      for (let i = 0; i< this.dates.length; i++) {
        naviges.push(this.dates[i])
      }
      return naviges
    }
    this.nows = function () {
      let nowlist = {}
      nowlist = this.dates
      return nowlist
    }
  }
</script>
