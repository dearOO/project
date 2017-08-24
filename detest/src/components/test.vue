<template>
  <div style="position:relative;top:-46px;">
    <div class="header">
        <div class="img">
          <x-img v-if="iconFlag" :src='usericon'></x-img>
          <input type="file" id="upload" @change="uploadImg()" accept="image/jpeg,image/png" capture='camera' ref="input">
        </div>
        <p>{{user}}</p>
    </div>
    <div class="contmy">
      <group>
        <cell :title="$t('vux.app_56')" is-link @click.native="clear"></cell>
        <popup-picker show-name :title="$t('vux.app_57')" v-model="lang" @on-hide="setLang" :confirm-text="$t('vux.app_19')" :cancel-text="$t('vux.app_20')" :data="langList"></popup-picker>
      </group>
    </div>
    <div class="btn">
      <button @click="exit()">{{$t('vux.yyaqtcmc')}}</button>
    </div>
  </div>

</template>
<script>
import Vue from 'vue'
import Masker from 'vux/src/components/masker/index.vue'
import Blur from 'vux/src/components/blur/index.vue'
import Group from 'vux/src/components/group/index.vue'
import Cell from 'vux/src/components/cell/index.vue'
import PopupPicker from 'vux/src/components/popup-picker/index.vue'
import XImg from 'vux/src/components/x-img/index.vue'

import axios from 'axios'
import router from '.././routes'
const qs = require('querystring');
const base64 = require('crypto-js/enc-base64');
const utf8 = require('crypto-js/enc-utf8');

export default {
  components: {
    Masker,
    Blur,
    Group,
    Cell,
    PopupPicker,
    XImg
  },
  data(){
    return {
      user: window.USERNAME,
      usericon: window.initJson.baseUrl+"/app/api/userInfo/getUserZp?userId=" + base64.stringify(utf8.parse('uid_' + window.USERID)) + '&t=' + Math.random(),
      iconFlag: true,
      CNlangList: [[{
        name: '中文',
        value: 'CN'
      },{
        name: 'English',
        value: 'EN'
      },{
        name: 'русский',
        value: 'BE'
      }]],
      BElangList: [[{
        name: 'китайский',
        value: 'CN'
      },{
        name: 'на английском языке',
        value: 'EN'
      },{
        name: 'русский',
        value: 'BE'
      }]],
      ENlangList: [[{
        name: 'Chinese',
        value: 'CN'
      },{
        name: 'English',
        value: 'EN'
      },{
        name: 'Russian',
        value: 'BE'
      }]],
      lang: []
    }
  },
  methods : {
    exit(){
      this.$cookie.set('TOKEN', undefined , { expires: '2h' });
      router.push({path: '/login'})
    },
    uploadImg(){
      var _this = this;
      if(!_this.$refs.input){
          _this.$vux.toast.show({
              type: 'cancel',
              text: this.$t('vux.app_05')
            })
          return;
      }
      if(_this.$refs.input.files.length){
        if(_this.$refs.input.files[0].size > 4 *1024 * 1024){
          _this.$refs.input.value = null;
          _this.$vux.toast.show({
              type: 'cancel',
              text: this.$t('vux.app_72')
            })
          return;
        }
        var file = _this.$refs.input.files[0];
        let param = new FormData(); //创建form对象
        param.append('zp',file,file.name);//通过append向form对象添加数据
        axios({
          method: 'post',
          url: '/app/api/userInfo/uploadUserZp',
          data: param,
          headers:{'Content-Type':'multipart/form-data'}
        }).then(function (resp){
          if(resp.data.result){
            _this.$nextTick(() => {
                _this.iconFlag = false;
                setTimeout(function() {
                    _this.usericon = window.initJson.baseUrl+"/app/api/userInfo/getUserZp?userId=" + base64.stringify(utf8.parse('uid_' + window.USERID)) + '&t=' + Math.random();
                    _this.iconFlag = true;
                }, 300);
            })
            _this.$refs.input.value = null;
          }
        })
        .catch();
      }
    },
    clear(){
      var _this = this;
        axios({
          method: 'post',
          url: '/app/api/clear'
        }).then(function(resp){
          if(resp.data.result){
            _this.$vux.toast.show({
              text: _this.$t('vux.app_12'),
              type: 'success',
            })
            axios({
              method: 'post',
              url: '/app/api/getLanguage',
            }).then(function (resp){
              if(resp.data.result){
                  // 赋值给i18n
                  for (let i in resp.data.data) {
                    Vue.i18n.add(i, resp.data.data[i])
                  }
                }
              })
              .catch(function (error) {
              });
          }
        }).catch()
    },
    setLang(closeType){
      var _this = this;
      if(closeType){
        axios({
          method: 'post',
          url: '/app/api/settingLanguage',
          data: qs.stringify({
            language : _this.lang[0]
          })
        }).then(function(resp){
          if(resp.data.result){
              Vue.i18n.set(_this.lang[0]);
              window.LANGUAGE = _this.lang[0];
              jy.preferencePut('LANG',_this.lang[0]);
          }
        }).catch()
      }
    }
  },
  created(){
    this.langList = this['CNlangList'];
    this.lang = [window.LANGUAGE];
  }
}
</script>

<style lang="less" scoped>
#upload{
  display: inline-block;
  width: 100px;
  height: 100px;
  border-radius: 50%;
  overflow: hidden;
  position: absolute;
  top: 0;
  left: 0;
  opacity: 0;
}
.header{
  text-align: center;
  padding-top: 60px;
  width: 100%;
  height: 200px;
  background: url(.././assets/icons/my.png);
  background-size: 100% 100%; 
  .img{
    width: 100px;
    height: 100px;
    border-radius: 50%;
    border: 5px solid #c2cfd1;
    overflow: hidden;
    margin: 0 auto;
    position: relative;
    img{
      width: 100%;
      height: 100%;
    }
  }
  p{
    color: white;
    margin-top: 10px; 
  }
}
.cont{
  .weui-cells::before,.weui-cells::after{
    display: none;
    border: none;
  }
  .vux-no-group-title{
    margin-top: 0 !important;
  }
}
.btn{
  padding: 0 15px;
  width: 100%;
  box-sizing: border-box;
  position: absolute;
  bottom: 0;
  left: 0;
  button{
    width: 100%;
    line-height: 40px;
    background: #0f5888;
    color: white;
    border: none;
    outline: none;
    border-radius: 5px;
    font-size: 16px;
  }
}
</style>
<style lang="less">
.contmy{
  .weui-cells::before{
    display: none;
    border: none;
  }
  .vux-no-group-title{
    margin-top: 0 !important;
  }
  .weui-cell__ft::after{
    display: none !important;
  }
  .vux-popup-picker-value{
    color: #0f5888;
    font-size: 14px;
  }
}
</style>

