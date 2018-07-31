/*!
* user-info
*
* 基本信息页
*
* @author xinjianan
* @version 1.0.0 20180727
*/
<template>
  <div class="page-wrapper top-padding">
    <form @submit.prevent="submit">
      <div class="out-wrapper">
        <div class="in-wrapper bottom-border usual-wrapper">
          <div class="usual-text black-text">服务类型</div>
        </div>
      </div>

      <div class="out-wrapper bottom-margin">
        <div class="in-wrapper bottom-border usual-wrapper" @click="selectShow()">
          <div class="usual-text blue-text float-left text-left">请选择</div>
          <div class="row-start vertical-center float-left text-right">
            <span id="selStyle"></span>
            <img src="./consult_submit/Path.png" class="selStyle2"/>
          </div>
          <div class="clear"></div>
        </div>
      </div>

      <div class="out-wrapper top-margin">
        <div class="in-wrapper bottom-border usual-wrapper">
          <div class="usual-text black-text">联系方式</div>
        </div>
      </div>

      <div class="out-wrapper bottom-margin">
        <div class="in-wrapper bottom-border usual-wrapper">
          <input type="text" class="usual-text input-phone" id="inputPhone" placeholder="+86  请输入您的电话号码">
        </div>
      </div>

      <div class="sub-wrapper">
        <div class="sub" @click="handleClickRefer">确认提交</div>
      </div>
    </form>

    <!--选择服务类型-->
    <div class="page-wrapper-select" id="sel">
      <div class="out-wrapper">
        <div class="in-wrapper-select bottom-border">
          <div class="usual-text float-left1 text-left">选择服务类型</div>
          <div class="float-right1 text-right">
            <img class="img-close" src="./consult_submit/close.png" @click="selectHide()"/>
          </div>
          <div class="clear"></div>
        </div>
        <div class="in-wrapper-select1 bottom-border left-margin" v-for="(item, index) in defaultIndustry" @click="handleSelect(index)">
          <div class="gray-text float-left1 usual-text black-text">
            <img :src="item.img"/>
            <span>{{item.name}}</span>
          </div>
          <div class="float-right1 select-wrapper">
            <div class="select-box" v-if="item.checked"></div>
          </div>
          <div class="clear"></div>
        </div>
      </div>
      <div class="bottom-position" :show="true">
        <div @click="handleConfirm" class="bottom-btn" :class="[isButtonShow ? 'abled-bg' : 'disabled-bg']">
          <div class="bottom-btn-text">确定</div>
        </div>
      </div>
    </div>
    <!--提示不正确消息-->
    <div class="tip" id="tip">
      <img src="./consult_submit/tip.png">
      <div class="tipClose" @click="handKnow()"></div>
    </div>

  </div>
</template>

<script>
  export default{
    data() {
      return {
        selectName: null,
        isButtonShow: false,
        defaultIndustry: [{
          img: require('./consult_submit/icon1.png'),
          name: '平面设计',
          checked: false
        }, {
          img: require('./consult_submit/icon2.png'),
          name: '包装设计',
          checked: false
        }, {
          img: require('./consult_submit/icon3.png'),
          name: '网站/APP设计',
          checked: false
        }, {
          img: require('./consult_submit/icon4.png'),
          name: '网站建设',
          checked: false
        }, {
          img: require('./consult_submit/icon5.png'),
          name: '空间设计',
          checked: false
        }, {
          img: require('./consult_submit/icon6.png'),
          name: '品牌策划',
          checked: false
        }, {
          img: require('./consult_submit/icon7.png'),
          name: '商标注册',
          checked: false
        }, {
          img: require('./consult_submit/icon8.png'),
          name: '卡通形象设计',
          checked: false
        }, {
          img: require('./consult_submit/icon9.png'),
          name: '视频拍摄',
          checked: false
        }, {
          img: require('./consult_submit/icon10.png'),
          name: '景观规划',
          checked: false
        }, {
          img: require('./consult_submit/icon11.png'),
          name: '其他',
          checked: false
        }]
      }
    },
    methods: {
      selectShow(){
        var sel = document.getElementById("sel");
        sel.style.display = 'block';
      },
      selectHide(){
        var sel = document.getElementById("sel");
        sel.style.display = 'none';
      },
      handleSelect(cur) {
        this.defaultIndustry.forEach((value, index) => {
          if (index === cur) {
            value.checked = !value.checked
            this.selectName = value.checked ? value.name : null
            this.isButtonShow = value.checked
          } else {
            value.checked = false
          }
        })
      },
      handleConfirm(){
        if(this.isButtonShow){
          var selStyle = document.getElementById("selStyle");
          selStyle.innerHTML = this.selectName;
          this.selectHide();
        }else{
          this.selectShow();
        }
      },
      handleClickRefer(){
        var inputPhone = document.getElementById("inputPhone").value;
        if(inputPhone != "" && !(/^1[34578]\d{9}$/.test(inputPhone))){
          var tip = document.getElementById("tip");
          tip.style.display = "block";
        }else if(inputPhone == ""){
          var tip = document.getElementById("tip");
          tip.style.display = "block";
        }else{
          alert("提交成功")
          fetch('http://120.27.135.162/qcbang-rest/product-type-group').then(r =>
            r.json()
          ).then(res =>{
            console.log(
              1);
            alert("提交成功")
          })
        }
      },
      handKnow(){
        var tip = document.getElementById("tip");
        tip.style.display = "none";
      }
    }
  }
</script>


<style scoped>
  .clear{
    clear: both;
  }
  .page-wrapper {
    width: 750px;
    height: 1334px;
    position: relative;
    background-color: #f6f6f6;
  }

  .out-wrapper {
    width: 750px;
    box-sizing: border-box;
    background-color: #fff;
  }

  .top-padding {
    padding-top: 20px;
  }
  .bottom-margin{
    margin-bottom: 20px;
  }

  .in-wrapper {
    width: 100%;
    padding-left: 50px;
    padding-right: 50px;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }

  .usual-wrapper {
    height: 98px;
    line-height: 98px;
  }

  .row-start {
    flex-direction: row;
    justify-content: flex-start;
  }

  .bottom-border {
    border-bottom-width: 1px;
    border-bottom-style: solid;
    border-bottom-color: #eee;
  }


  .vertical-center {
    align-items: center;
  }

  .usual-text {
    font-size: 32px;
  }

  .black-text {
    color: #333333;
  }

  .blue-text{
    color: #64c8fd;
  }

  .float-left{
    width: 50%;
    float: left;
  }
  .text-left{
    text-align: left;
  }
  .text-right{
    text-align: right;
  }

  #selStyle{
    padding-right: 10px;
    font-size: 30px;
    color: #333333;
  }
.selStyle2{
	display: inline-block;
    font-size: 30px;
  }
  
  .input-phone{
    width: 100%;
    border: none;
    -webkit-appearance: none;
    outline: none;
  }
  .sub-wrapper{
    width: 100%;
    margin-top: 58px;
    text-align: center;
  }
  .sub{
    width: 90%;
    height: 90px;
    padding: 2% 0;
    margin: 0 auto;
    font-size: 36px;
    color: #ffffff;
    border-radius: 45px;
    border: none;
    background-image: url(./consult_submit/subBg.png);
    background-repeat: no-repeat;
    background-size: 100%;
    -webkit-appearance: none;
    outline: none;
  }

  /*选择服务类型*/
  .page-wrapper-select{
    width: 750px;
    height: 100%;
    position: absolute;
    bottom: 0;
    background-color: #ffffff;
    display: none;
  }
  .page-wrapper-select1{
    width: 750px;
    height: 100%;
    position: absolute;
    bottom: 0;
  }
  .float-left1{
    width: 80%;
    float: left;
  }
  .float-right1{
    width: 20%;
    float: right;
  }
  .left-margin{
    margin-left: 48px;
  }
  .in-wrapper-select{
    width: 750px;
    height: 90px;
    line-height: 90px;
    padding-left: 50px;
    padding-right: 50px;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
  .in-wrapper-select1{
    width: 702px;
    height: 90px;
    line-height: 90px;
    padding-right: 48px;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
  .select-wrapper{
    width: 32px;
    height: 32px;
    margin-top: 29px;
    border-width: 2px;
    border-style: solid;
    border-color: #62c7fc;
    border-radius: 16px;
    box-sizing: content-box;
  }
  .select-box {
    width: 20px;
    height: 20px;
    margin: 6px;
    border-radius: 10px;
    background-color: #1098f7;
  }
  .gray-text {
    color: #999;
  }
  .bottom-btn {
    width: 90%;
    height: 90px;
    margin: 2% auto;
    padding: 2% 0;
    font-size: 36px;
    color: #ffffff;
    text-align: center;
    border-radius: 45px;
    border: none;
    background-image: url(./consult_submit/subBg.png);
    background-repeat: no-repeat;
    background-size: 100%;
    -webkit-appearance: none;
    outline: none;
  }
  .bottom-btn-text {
    font-size: 36px;
    color: #fff;
  }
  .img-close{
    width: 32px;
    height: 32px;
  }
  .bottom-position{
    width: 100%;
    position: absolute;
    bottom: 0;
  }
  .disabled-bg {
    opacity: 0.5;
  }

  .abled-bg {
    opacity: 1;
  }


  /*提示消息*/
  .tip{
    position: absolute;
    top: 0;
    width: 100%;
    height: 100%;
    text-align: center;
    padding-top: 50%;
    background-color: rgba(0,0,0,0.3);
    display: none;
  }
  .tip img{
    width: 76%;
  }
  .tipClose{
    position: absolute;
    top: 51%;
    left: 12%;
    width: 76%;
    height: 95px;
    /*border: 1px solid red;*/
  }
</style>

