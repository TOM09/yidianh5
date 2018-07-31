


 /*! * Product Detail Vue * * 产品详情页(案例) *

  * @author songhua * @version 1.0.0 20180727

  */

<template>
	<div>
		<div>
			<!--<div ref="top" />-->
			<topLogo2/>

			<div class="shadowSpace123" />


			<!--<video v-if="productDetailObject.materielType" :src='productDetailObject.imgs' class="banner" />-->
			<video  v-if="productDetailObject.materielType" id="myvideo" :src="productDetailObject.imgs" :muted="muteStatus" controls>

    		</video>

			<vueswiper :imgData="imgData" />

			<!--<div v-else class="banner" interval="4000" auto-play="true">-->
			<!--<img v-for="item in productDetailObject.imgs.split(',')" :src="item" />-->
			<!--<indicator v-if="productDetailObject.imgs.split(',').length > 1" class="indicator" />-->
			<!--</div>-->

			<div class="detail-title-container">
				<div class="detail-left-container">
					<img class="detail-title-image" :src="typeIconMap[productDetailObject.productType]" />
					<div class="detail-describe">
						<span class="detail-title">{{typeNameMap[productDetailObject.productType]}}</span>
						<span class="detail-level">{{productDetailObject.productGradeName}}</span>
					</div>

				</div>

				<!--<div class="like-wrapper" @click="handleClickLike(productDetailObject, caseLikeStatusList[productDetailObject.id] ? false : true)">
        <img class="like" v-if="caseLikeStatusList[productDetailObject.id]" :src="like" />
        <img class="like" v-else :src="unLike" />
      </div>-->
			</div>

			<div class="detail-info-container">
				<div class="detail-info-left">
					<span class="info-title">{{productDetailObject.name}}</span>
					<div>
						<span class="info-time">{{productDetailObject.label}}</span>
					</div>
				</div>

				<!--<div class="shadowSpaceSolid" />-->
<!--
				<div class="detail-info-right">
					<div class="preview-container">
						<img class="preview-image" :src="preview" />
						<span class="preview-text">{{productDetailObject.browseNum}}</span>
					</div>
					<div class="praise-container">
						<img class="praise-image" :src="praise" />
						<span class="praise-text">{{productDetailObject.likeNum}}</span>
					</div>
				</div>-->

			</div>

			<div class="background-idea-container">
				<div class="background-idea-title">
					<img class="background-image" :src="background" />
					<span class="background-text">{{productDetailObject.detailKey1}}</span>
				</div>
				<div v-for="item in String(this.productDetailObject.detailVal1).split('\n')" class="background-describe">
					<span class="describe-text">{{item}}</span>
				</div>
				<div class="background-idea-title">
					<img class="idea-image" :src="idea" />
					<span class="idea-text">{{productDetailObject.detailKey2}}</span>
				</div>
				<!--项目背景-->
				<div v-for="item in String(this.productDetailObject.detailVal2).split('\n')" class="background-describe">
					<span class="describe-text">{{item}}</span>
				</div>
			</div>

			<div class="shadowSpace" />

			<!--服务流程-->

			<serviceProcess v-if="typeObject.flowId === 'design_flow'" type="design" />
			<serviceProcess v-if="typeObject.flowId === 'video_flow'" type="video" />
			<trademarkServiceProcess v-if="typeObject.flowId === 'registration_of_trade_mark_flow'" />

			<div class="shadowSpace" />

			<!--服务内容与规范-->
			<refundsRule :refundsRule="typeObject.refundsRule" />

			<!--猜你喜欢-->
			<div v-if="greatCaseList.length !== 0" class="custom-service-title">
				<img class="custom-service-image" :src="customService" />
				<span class="custom-service-text">猜你喜欢</span>
			</div>

			<div v-if="greatCaseList.length !== 0" class="case-title-container">
				<span class="case-title">{{groupListMap[productDetailObject.productGroup]}}作品示例</span>
			</div>

			<div v-for="(item, index) in greatCaseList">
				<div class="great-case-describe" @click="handleClickJump(item)">
					<div class='great-case-img-container' :style="{ 'margin-top': 14 }">
						<img class="great-case-img" :src="item.cover.split(',')[0]" />
						<img class="great-case-img" :src="item.cover.split(',')[1]" />
					</div>
					<span class="great-case-title-text">{{item.name}}</span>
					<span v-if='item.detailVal1.length > 33' class="great-case-detail-text">{{item.detailKey1}}：{{item.detailVal1.substring(0, 33)}}...</span>

					<span v-else class="great-case-detail-text">{{item.detailKey1}}：{{item.detailVal1}}</span>

					<!--<div class="like-container">
          <img v-if="caseLikeStatusList[item.id]" class="like" :style="{'margin-right':16}" :src="like" @click="handleClickLike(item, false)" />
          <img v-else class="like" :src="unLike" :style="{'margin-right':16}" @click="handleClickLike(item, true)" />
          <span class="like-number">{{item.likeNum}}</span>
        </div>-->

					<div v-if="index < greatCaseList.length - 1" class="col-line" />
				</div>
			</div>

			<div class="shadowSpace" />
			<div class="shadowSpace111" />

			<bottomBanner/>

		</div>
	</div>
</template>
<script>
	import bottomBanner from './product/bottomBanner.vue'
	import topLogo2 from './design/topLogo2'

	import {
		imageUrls,
		baseUrl
	} from '../lib/constants'

	import {
		RandomUtil
	} from '../lib/util'

	import vueswiper from './product/vueswiper'
	import serviceProcess from './product/service-process-component.vue'
	import trademarkServiceProcess from './product/trademark-service-process-component.vue'
	import refundsRule from './product/service-content-component.vue'

	export default {

		components: {
			serviceProcess,
			trademarkServiceProcess,
			refundsRule,
			vueswiper,
			bottomBanner,
			topLogo2
		},

		data() {
			return {
				_dom: "",
				playStatus: '',
				muteStatus: '',
				isMute: true,
				isPlay: false,
				imgData: [],
				baseUrl: baseUrl,
				typeNameMap: {},
				typeIconMap: {},
				typeObject: {},
				greatCaseList: [],
				isBuy: false,
				showLoading: 'hide',
				scrollerLoading: false,
				loading: imageUrls.navigation.loading,
				background: imageUrls.navigation.background,
				idea: imageUrls.navigation.idea,
				preview: imageUrls.navigation.preview,
				banner: imageUrls.banner.record,
				praise: imageUrls.navigation.praise,
				like: imageUrls.navigation.like,
				unLike: imageUrls.navigation.unLike,
				productDetailObject: this.$route.params.productDetailObject,
				groupListMap: this.$route.params.groupListMap,
				customService: imageUrls.navigation.customService
			}
		},

		created: function() {
			this.getList()

		},

		methods: {
			getList: function() {
				this.axios.get(this.baseUrl + '/product-type').then((response) => {
					console.log(response)
					var responseData = response.data.data
					for(const item of responseData) {
						this.typeNameMap[item.id] = item.name
						this.typeIconMap[item.id] = item.img
						if(item.id === this.productDetailObject.productType) this.typeObject = item
					}
				})

				this.axios.get(this.baseUrl + '/case?productGroupId=' + this.productDetailObject.productGroup).then((response) => {
					console.log(response)
					var responseData = response.data.data
					this.greatCaseList = responseData.filter((val) => {
						return !val.delFlag && val.status && val.id !== this.productDetailObject.id
					})
					this.greatCaseList = RandomUtil.getArray(this.greatCaseList, 6)
				})

				this.imgData = this.productDetailObject.imgs.split(',')

			},
			//
			//  handleClickJump: function(item) {
			//    NetworkUtil.streamRequest({
			//      method: 'POST',
			//      type: 'json',
			//      url: this.baseUrl + '/case/browse',
			//      headers: {
			//        'Content-Type': 'application/json'
			//      },
			//      body: {
			//        id: item.id,
			//        updateBy: this.userInfo.dingId
			//      }
			//    }, response => {
			//      this.productDetailObject = item
			//    })
			//    this.getList()
			//    ScrollUtil.scrollToElement(this.$refs.top, {})
			//  },

			handleBuyClick: function() {
				this.isBuy = true
			},

			handleBuyClose: function() {
				this.isBuy = false
			}
		}
	}
</script>
<style scoped>
	#myvideo{
    width: 100%;
  }
	.wrapper22 {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		width: 750px;
		height: 1112px;
	}

	.x-wrapper {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		width: 750px;
		height: 1350px;
	}

	.indicator {
		width: 750px;
		height: 586px;
		item-color: rgba(0, 0, 0, 0.2);
		item-selected-color: #1098f7;
		item-size: 20px;
		bottom: -250px;
		left: 0;
		opacity: 0.7;
		item-size: 12px;
		position: absolute;
	}

	.like-wrapper {
		width: 122px;
		height: 118px;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
	}

	.like-number {
		font-size: 28px;
		color: #666666;
	}

	.banner {
		width: 750px;
		height: 586px;
	}

	.detail-title-container {
		display: flex;
		flex-direction: row;
		width: 750px;
		height: 168px;
		justify-content: space-between;
		align-items: center;
	}

	.detail-info-left {
		display: flex;
		flex-direction: column;
		justify-content: center;
		margin-left: 20px;
	}

	.info-title {
		font-size: 44px;
		color: #333333;
		font-weight: bold;
	}

	.info-time {
		font-size: 26px;
		color: #999999;
		margin-top: 50px;
	}

	.detail-describe {
		display: flex;
		flex-direction: column;
		margin-left: 34px;
	}

	.detail-title {
		font-size: 26px;
		color: #333333;
	}

	.shadowSpace111 {
		background-color: #f6f6f6;
		width: 750px;
		height: 100px;
	}

	.detail-info-container {
		border-top: 1px solid #F6F6F6;
		border-bottom: 1px solid #F6F6F6;
	}

	.shadowSpaceSolid {
		background-color: #f6f6f6;
		width: 750px;
		height: 2px;
	}

	.detail-level {
		margin-top: 18px;
		font-size: 26px;
		color: #666666;
	}

	.detail-title-image {
		width: 68px;
		height: 68px;
	}

	.detail-left-container {
		display: flex;
		flex-direction: row;
		margin-left: 20px;
	}

	.detail-info-right {
		display: flex;
		flex-direction: row;
		align-items: flex-end;
		margin-bottom: 40px;
	}

	.preview-container {
		display: flex;
		flex-direction: row;
		margin-right: 30px;
	}

	.praise-container {
		display: flex;
		flex-direction: row;
		margin-right: 20px
	}

	.preview-image {
		width: 36px;
		height: 26px;
		margin-right: 12px;
	}

	.preview-text {
		font-size: 26px;
		color: #999999;
	}

	.praise-text {
		font-size: 26px;
		color: #999999;
	}

	.praise-image {
		width: 28px;
		height: 28px;
		margin-right: 12px;
	}

	.background-idea-container {
		display: flex;
		flex-direction: column;
		border-top-width: 1px;
		border-top-color: #eeeeee;
		margin-bottom: 60px;
	}

	.background-idea-title {
		display: flex;
		flex-direction: row;
		margin-left: 20px;
		margin-top: 60px;
		align-items: flex-end;
	}

	.background-image {
		width: 36px;
		height: 46px;
	}

	.background-text {
		margin-left: 32px;
		font-size: 32px;
		color: #333333;
		font-weight: bold;
	}

	.background-describe {
		display: flex;
		flex-direction: column;
		margin-left: 20px;
		margin-right: 20px;
	}

	.describe-text {
		line-height: 46px;
		font-size: 30px;
		color: #666666;
		margin-top: 52px;
	}

	.idea-image {
		width: 43px;
		height: 46px;
	}

	.idea-text {
		margin-left: 26px;
		font-size: 32px;
		color: #333333;
		font-weight: bold;
	}

	.detail-info-container {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		border-top-width: 1px;
		border-top-color: #eeeeee;
		height: 208px;
	}

	.footer {
		display: flex;
		flex-direction: row;
		height: 98px;
		width: 750px;
	}

	.footer-left {
		display: flex;
		flex-direction: row;
		height: 98px;
		width: 480px;
		align-items: center;
		background-color: white;
	}

	.footer-like {
		width: 46px;
		height: 42px;
		margin-left: 30px;
	}

	.footer-right {
		display: flex;
		flex-direction: row;
		height: 98px;
		width: 270px;
		background-color: #0f98f8;
		justify-content: center;
		align-items: center;
	}

	.footer-left-text-like {
		color: #666666;
		font-size: 32px;
		margin-left: 178px;
	}

	.footer-left-text {
		color: #999999;
		font-size: 32px;
		margin-left: 178px;
	}

	.footer-right-text {
		font-size: 32px;
		color: #ffffff;
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

	.custom-service-title {
		display: flex;
		height: 98px;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		background-color: #f6f6f6;
	}

	.custom-service-image {
		width: 44px;
		height: 38px;
	}

	.custom-service-text {
		margin-left: 18px;
		font-size: 32px;
		color: #333333;
		font-weight: bold;
	}

	.great-case-describe {
		display: flex;
		flex-direction: column;
		margin-top: 30px;
	}

	.great-case-img-container {
		display: flex;
		flex-direction: row;
		justify-content: space-between;
	}

	.great-case-img {
		width: 364px;
		height: 322px;
	}

	.great-case-img-full {
		width: 750px;
		height: 322px;
	}

	.great-case-title-text {
		color: #333333;
		font-size: 28px;
		margin-top: 40px;
		margin-left: 20px;
	}

	.great-case-detail-text {
		color: #666666;
		font-size: 28px;
		margin-top: 26px;
		margin-left: 20px;
		margin-right: 46px;
	}

	.case-title-container {
		display: flex;
		flex-direction: row;
		align-items: center;
		height: 98px;
		border-bottom-width: 1px;
		border-bottom-color: #eeeeee;
	}

	.case-title {
		margin-left: 20px;
		font-size: 32px;
		color: #333333;
		font-weight: bold;
	}

	.like-container {
		display: flex;
		flex-direction: row;
		justify-content: flex-end;
		margin-right: 20px;
		margin-bottom: 36px;
	}

	.like {
		width: 42px;
		height: 38px;
	}

	.col-line {
		width: 710px;
		margin-left: 20px;
		border-width: 1px;
		border-color: #eeeeee;
	}

	.loading {
		margin-left: 300px;
		width: 62px;
		height: 84px;
	}

	.loading-container {
		height: 100px;
		background-color: #f6f6f6;
		width: 750px;
		display: flex;
		flex-direction: row;
		align-items: flex-end;
	}

	.loading-text {
		font-size: 22px;
		color: #999999;
		margin-bottom: 10px;
	}

	.mask {
		height: 1100px;
		width: 750px;
		position: absolute;
		left: 0;
		top: 0;
		background-color: rgba(0, 0, 0, 0.4)
	}
</style>
