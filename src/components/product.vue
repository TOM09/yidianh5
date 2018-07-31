


<!--二级类目详情页-->


<template>
<div class="'wrapper">
	<topLogo :urlname = "productTypeData.name"/>
    <div class="shadowSpace123" />

    <div class="shadowSpace" />

    <div class="banner" interval="4000" auto-play="true">
      <img v-for="item in productTypeData.banners.split(',')" :src="item" />
      <!--<indicator class="indicator" />-->
    </div>

    <div class="shadowSpace" />

    <div>
      <div v-if="productTypeData.name != ''" class="title">
        <div class="title-left-container">
          <img class="nav" :src="brandNav" />
          <span class="product-text">  {{productTypeData.name}}</span>
        </div>

        <div v-if="productTypeData.subTitle" class="title-right-container" @click="handleClickVIDetail">
          <span class="title-right">整套VI设计列表参考</span>
          <img class="right-image" :src="rightArrow" />
        </div>
      </div>

      <div v-for="item in designerList">
        <designer :designerObject="item" :productTypeData="productTypeData" :discountList="discountList" />
      </div>
    </div>


    <div class="shadowSpace" />

 	<div>
      <process v-if="productTypeData.flowId === 'design_flow'" type="design" />
      <process v-else-if="productTypeData.flowId === 'video_flow'" type="video" />
      <trademarkServiceProcess v-else-if="productTypeData.flowId === 'registration_of_trade_mark_flow'" />
      <div class="shadowSpace" />
    </div>

    <refundsRule :refundsRule="productTypeData.refundsRule" />

    <bottomBanner/>
    <div class="shadowSpace111" />

</div>
</template>
<script>
import {
  imageUrls,
  baseUrl
} from '../lib/constants'

import process from './product/service-process-component.vue'
import trademarkServiceProcess from './product/trademark-service-process-component.vue'
import designer from './product/designers-component.vue'
import refundsRule from './product/service-content-component.vue'
import topLogo from './product/topLogo'
import bottomBanner from './product/bottomBanner.vue'

export default {
	name: 'product',
	components: {
	    process,
	    designer,
	    topLogo,
	    refundsRule,
	    trademarkServiceProcess,
	    bottomBanner
	  },
	data: function() {
	    return {
		productTypeData: this.$route.params.productTypeData,
	      consultSuspend: imageUrls.navigation.consultSuspend,
//	      designerListCacheName: cacheName.designer.designerList,
	      baseUrl: baseUrl,
	      designerList: [],

	      discountList: [],
	      brandNav: imageUrls.navigation.brand,
	      rightArrow: imageUrls.navigation.rightArrow,
	      isBuy: false
	    }
  },
  created: function() {


  this.axios.all([this.getInfo().productMasterpiece,this.getInfo().productOne, this.getInfo().productTwo,this.getInfo().productThree])
				.then(this.axios.spread((acct, perms, res) => {
					var acctData = acct.data.data;
				if (acctData) {
			        var now = new Date().getTime()
			        this.discountList = acctData.filter((val) => {
			          if (val.endDate && val.quantity && !val.delFlag && val.status === 1 && now > val.startDate && now < val.endDate && val.quantity - val.sold > 0) return true
			          if (!val.quantity && !val.endDate && val.status === 1 && !val.delFlag) return true
			          if (!val.quantity && now > val.startDate && now < val.endDate && val.status === 1 && !val.delFlag) return true
			          if (!val.endDate && val.quantity - val.sold > 0 && !val.delFlag && val.status === 1) return true
			        })
			      }


			     var response = perms.data.data
			     this.designerList = response.filter((val) => {
			        return !val.delFlag && val.detailid === this.productTypeData.detailId
			      })
			      this.designerList.sort((indexA, indexB) => {
			        if (indexA.grade > indexB.grade) return 1
			        return -1
			      })

    }))
},
  methods:{
	  getInfo(){
		let obj = {
	          productMasterpiece: this.axios.get(this.baseUrl + '/product-discount?productGroup=' + this.productTypeData.groupid + '&productType=' + this.productTypeData.id),
			productOne: this.axios.get(this.baseUrl+'/product-designer')
//			productTwo: this.axios.get(this.baseUrl+'/product-type-group?id='+this.urlId),
//			productThree: this.axios.get(this.baseUrl+'/case?productGroupId='+this.urlId)
				}
			return obj
		}
  }





	}

</script>
<style scoped>
	.wrapper {
  display: flex;
  flex-direction: column;
  width: 750px;
  justify-content: space-between;
  height: 1112px;
}

.x-wrapper {
  display: flex;
  flex-direction: column;
  width: 750px;
  justify-content: space-between;
  height: 1350px;
}

.print-image-container {
  width: 750px;
  height: 330px;
}

.print-image {
  width: 750px;
  height: 350px;
}

.nav {
  margin-left: 20px;
  width: 10px;
  height: 32px;
}

.title {
  display: flex;
  flex-direction: row;
  height: 98px;
  justify-content: space-between;
}

.title-left-container {
  display: flex;
  flex-direction: row;
  height: 98px;
  align-items: center;
}

.banner {
  width: 750px;
  height: 340px;
}

.product-text {
  font-size: 32px;
  color: #333333;
}

.bottom-text {
  width: 750px;
  height: 98px;
  background-color: #1098f7;
  text-align: center;
  color: #ffffff;
  font-size: 32px;
  padding-top: 33px;
}

.segmentation {
  width: 750px;
  height: 40px;
}

.shadowSpace {
  background-color: #f6f6f6;
  width: 750px;
  height: 20px;
}

.shadowSpace123 {
  background-color: #f6f6f6;
  width: 750px;
  height: 80px;
}
.shadowSpace111 {
  background-color: #f6f6f6;
  width: 750px;
  height: 100px;
}

.title-right-container {
  display: flex;
  flex-direction: row;
  margin-right: 20px;
  margin-top: 40px;
}

.title-right {
  font-size: 26px;
  color: #666666;
}

.right-image {
  width: 12px;
  height: 22px;
  margin-top: 5px;
  margin-left: 14px;
}

.mask {
  height: 1100px;
  width: 750px;
  position: absolute;
  left: 0;
  top: 0;
  background-color: rgba(0, 0, 0, 0.4)
}

.consult-suspend {
  width: 124px;
  height: 124px;
}
</style>
