
<template>
     <v-header :seller="seller"></v-header> 
     <div class="tab border-1px">
      <div class="tab-item">
        <a v-link="{path:'/goods'}">商品</a>
      </div>
      <div class="tab-item">
        <a v-link="{path:'/ratings'}">评论</a>
      </div>
      <div class="tab-item">
        <a v-link="{path:'/seller'}">商家</a>
      </div>
    </div>
	<div>
		<router-view :seller="seller"></router-view>
	</div>
</template>

<script type="text/ecmascript-6">
const ERR_OK=0;
import header from 'components/header/header.vue';
import {urlParse} from 'common/js/util';

 export default {
  data(){
    return {
      seller: {
      	id: (() => {//立即执行函数，通过url获取账户id
      		let queryParam = urlParse();
      		console.log(queryParam);
      		return queryParam.id;
      	})()
      }
    };
  },

  created(){ 
    this.$http.get('/api/seller?id='+this.seller.id).then((response) => {
    	response=response.body;
    if(response.errno===ERR_OK)
    	//拓展对象的属性
    	this.seller = Object.assign({},this.seller,response.data);
    })
  },
      components: {
      'v-header': header
      }
  };

</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "./common/stylus/mixin.styl"
   .tab
      display: flex
      width: 100%
      height: 40px
      line-height: 40px
      border-1px(rgba(7, 17, 27, 0.1))
      .tab-item
        flex: 1
        text-align: center
        & > a
          display: block
          font-size: 14px
          color: rgb(77, 85, 93)
          &.active
            color: rgb(240, 20, 20)
</style>
