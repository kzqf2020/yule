<template>
    <el-container class="home-under-header">
      <el-main id="home-main" class="home-main">
        <keep-alive exclude="PlatformRooms,AreaAll">
          <router-view ref="mychild" @loginSuccess="loginSuccess" @startLoad="startLoad" @loadFinish="loadFinish" @activated="activated" :isLogin="isLogin" :userInfo="userInfo"></router-view>
        </keep-alive>
      </el-main>
      <el-footer class="home-aside">
        <el-row >
          <el-col id="aside1" :span="12">
            <router-link class="home-aside-item" to="/mobile/index/home/recommend">
              <div class="home-aside-item-icon"><i class="iconfont icon-home"></i></div>
              <div class="home-aside-item-words">推荐</div>
            </router-link>
          </el-col>
          <el-col id="aside2" :span="12">
            <router-link class="home-aside-item"  to="/mobile/index/home/follows">
              <div class="home-aside-item-icon"><i class="iconfont icon-favorite"></i></div>
              <div class="home-aside-item-words">关注</div>
            </router-link>
          </el-col>
<!--          <el-col id="aside3" :span="8">-->
<!--            <router-link class="home-aside-item" to="/mobile/index/home/areas">-->
<!--              <div class="home-aside-item-icon"><i class="iconfont icon-fenlei"></i></div>-->
<!--              <div class="home-aside-item-words">分区</div>-->
<!--            </router-link>-->
<!--          </el-col>-->
        </el-row>
        <div class="beside-aside"></div>
      </el-footer>
      <el-backtop target=".home-main"></el-backtop>
    </el-container>
</template>
<script>

import {getApp} from "@/api/liveList";

export default {
  name: 'HomeMobile',
  components: {

  },
  props: ['userInfo','isLogin'],
  data() {
    return {
      appUrl:"",
      player: null,
      isActive: false,
      searchInput: '',
      transformFlag: 'translateY(0px)',
      gettingList: false,
    }
  },
  methods: {
    toGithubIssuePage(){
      window.open("https://github.com/guyijie1211/JustLive-Web/issues/new", "_blank");
    },
    toAndroidAppRelease(){
      window.open("https://github.com/guyijie1211/JustLive-Android/releases");
    },
    load(){
      this.$refs.mychild.loadRoomList();
    },
    getAppUrl() {
      getApp()
          .then(response => {
            if(response.data.code === 200){
              let info = response.data.data
              this.appUrl = info.apkMD5
            }
          })
    },
    loginSuccess(userInfo){
      let _this = this
      _this.$emit("loginSuccess",userInfo)
    },
    clickRecommend(){
      let beside = document.getElementsByClassName("beside-aside")[0]
      beside.style.transform = 'translateX(0)'
    },
    clickFollows(){
      let beside = document.getElementsByClassName("beside-aside")[0]
      let left = document.getElementById("aside1").offsetWidth
      beside.style.transform = 'translateX('+left+'px)'
    },
    clickKinds(){
      let beside = document.getElementsByClassName("beside-aside")[0]
      let left = document.getElementById("aside2").offsetWidth
      beside.style.transform = 'translateX('+left*2+'px)'
    },
    activated(target){
      if (target == 0){
        this.clickRecommend()
      }
      if (target == 1){
        this.clickFollows()
      }
      if (target == 2){
        this.clickPlatform()
      }
      if (target == 3){
        this.clickKinds()
      }
      if (target == 4){
        this.clickTv()
      }
    },
    loadFinish() {
      this.gettingList = false;
    },
    listenerFunction(e) {

    },
    startLoad() {
      this.gettingList = true
    },
    handleScroll () {
      let target = document.getElementById("home-main")
      if(!this.gettingList && (target.scrollHeight-target.clientHeight)-target.scrollTop < 5){
        this.load()
      }
    },
    toTop(){

    },
  },
  activated() {
    console.log("activated")
    document.addEventListener('scroll', this.handleScroll, true);
  },
  created() {
    this.getAppUrl()
  },
  deactivated() {
    console.log("deactivated")
    document.removeEventListener('scroll', this.handleScroll, true);
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.home-under-header{
  height: 80%;
  width: 100%;
}
.home-aside{
  /*padding-top: 10px;*/
  width: 100%;
  box-shadow: 0px -5px 5px -5px #4e4c4c;
  overflow: hidden;
  position: relative;
}
.home-aside-item{
  /*padding-top: 10px;*/
  /*padding-bottom: 5px;*/
  float: top;
  width: 100%;
  height: 100%;
}
.home-aside-item-icon{
  margin-left: 30%;
  float: left;
}
.home-aside-item-words{
  font-weight: lighter;
  margin-left: 10px;
  float: left;
}

/deep/ .el-input__inner{
  height: 40px;
  border-radius: 20px;    /*输入框圆角值*/
}
a {
  color: #303133;
  text-decoration: #303133;
}
.home-main{
  overflow-x: hidden;
  overflow-y: auto;
}
.home-main::-webkit-scrollbar {
  width: 3px;
  /*height: 4px;*/
}
.home-main::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background: #8e8e8e;
}
.el-main/deep/ {
  padding: 10px;
}

.beside-aside{
  position: absolute;
  left: 9%;
  bottom: 2px;
  height: 6px;
  width: 33%;
  margin-left: 10px;
  background: #4e4c4c;
  transition: all 0.3s;
  transform: translateX(0px);
}
.el-header, .el-footer {
  color: #333;
  text-align: center;
  line-height: 60px;
}
</style>
