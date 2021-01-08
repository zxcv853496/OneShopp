<template>
  <div id="app">
    <div id="nav">
      <MainNav />
    </div>
    <router-view/>
    <profile :profile="profile"></profile>
    <button @click="login" v-if="!authorized">Login</button>
    <button @click="logout" v-else>Logout</button>
    <div class="g-signin2" data-onsuccess="onSignIn" data-theme="dark"></div>
    <a href="#" onclick="signOut();">Sign out</a>
  </div>
</template>



<script>
// @ is an alias to /src
import './assets/css/app.scss'
import MainNav from './components/MainNav/index'
import Profile from './components/Profile/index'



export default {
  name: 'app',
  components: {
    MainNav,
    Profile,
  },
  data(){
    return{
      profile: {},
      authorized: false
    }
  },
  mounted () {  //window的意思？
  // 防止重複載入
    
      let vm = this
      window.fbAsyncInit = function() {  //fb非同步初始化
        window.FB.init({                
          appId      : '450762172745973',
          cookie     : true,
          xfbml      : true,
          version    : 'v9.0'
        });
        window.FB.AppEvents.logPageView();  //好像跳轉頁面 不確定
        console.log('fbAsyncInit')

      window.FB.getLoginStatus( response => {   
        vm.statusChangeCallback(response)     
      // 這裡可以得到 fb 回傳的結果
      //其中比較重要的 status 包含幾個狀態{
        // connected: 使用者已登入 FB，且授權你的 app 使用。
        // not_authorized: 使用者已登入 FB，但未授權你的 app 使用。
        // unknown: 使用者沒有登入 FB，或已從你的 app 中登出。      }
        // 如果你拿不到 authResponse 有可能是使用者沒有登入 FB，或者他尚未授權你的 app 使用。
        })
      };
    
  },
  methods: {
  // FB.getLoginStatus()：獲得用戶的登入狀態
  // FB.login(callback, {scope})：讓用戶登入並授權，scope 可參考官方權限
  // FB.logout()：登出用戶
  // FB.api(path, method, params, callback)：因為 Facebook 為 Graph API，所以比較複雜一些，詳細可參考官方，這邊介紹幾個比較常用的
  // FB.api("/me?fields=name,id,email", callback)：獲取個人資料，參數接在問號後方，一樣可參考官方權限
  // FB.api("/me/permissions", "DELETE", callback)：刪除授權，用戶 status 須為 connected
  //登入
    login () {  
        let vm = this
        window.FB.login(function (response) { //取得狀態
          vm.getProfile() //取得個人資料
          vm.statusChangeCallback(response)
      }, {
        scope: 'email, public_profile',   //想取得的資訊
        return_scopes: true               //return_scopes 為 true 時可以得到被授權的清單
                                          //可以在使用者登入的 response 中多得到一個名為 grantedScopes 的屬性，裡面會告訴你授權了哪些可用的項目
      })
      
    },

  //登出
    logout () {   
      let vm = this
      // 檢查登入狀態
      window.FB.getLoginStatus(function(response) {
        // 檢查登入狀態
        if (response.status === "connected") {
          // 移除授權
          window.FB.api("/me/permissions", "DELETE", function() {
            // 用戶登出
          window.FB.logout(function(response) {
            vm.statusChangeCallback(response)
          });
          alert('登出成功')
          });
        } 
        else {
          alert('未登入')
        }
      });
    },

    statusChangeCallback (response) {
      let vm = this
      if (response.status === 'connected') {
        vm.authorized = true
        
      } 
      else if (response.status === 'not_authorized') {
        vm.authorized = false
      } 
      else if (response.status === 'unknown') {
        vm.profile = {}
        vm.authorized = false
      } 
      else {
        vm.authorized = false
      }
      vm.getProfile()
    },

  //這邊才能真正抓取到資料
    getProfile() {  
      let vm =this
      window.FB.api('/me?fields=name,id,email', function (response) {
        vm.$set(vm, 'profile', response)  //監控 profile
      })
    },
  }
}
</script>
