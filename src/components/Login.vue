<template>
  <div class="u-content" id="login">
    <img src="../assets/banner.jpeg">
    <div class="u-login">
      <img src="../assets/login.png">

      <div class="login-form">

        <div class="login-form-list">
          <div class="login-form-input">
            <input v-model="tel" class="input-mobile" type="tel" placeholder="手机号">
          </div>
        </div>

        <div class="login-form-list">
          <div class="login-form-yz">
            <input v-model="code" class="input-pwd" placeholder="验证码" type="text">
          </div>
          <div v-show="sendAuthCode" class="login-form-send" v-on:click="getCode">获取验证码</div>
          <div v-show="!sendAuthCode" class="login-form-send"> {{auth_time}}秒</div>
          <!--<input class="input-send" value="获取验证码">-->
          <!--</input>-->
        </div>

        <div class="login-form-list login-button" v-on:click="login">马上拿钱</div>

      </div>
    </div>
    <div>
      <img src="../assets/footer.png">
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  props: {
    msg: String
  },
  data(){
  return {
      sendAuthCode:true,
      tel: "",
      code: "",
      auth_time: 0
    }
  },
  methods: {
    getCode: function () {
      const self = this
      if (!(/^1[2345789]\d{9}$/.test(self.tel))){
         alert("请输入正确的手机号码")
       } else if (self.tel == "" || !self.tel) {
         alert("请输入手机号码")
       } else {
        this.sendAuthCode = false;
        this.auth_time = 60;
        var auth_timetimer =  setInterval(()=>{
        this.auth_time--;
        if(this.auth_time<=0){
          this.sendAuthCode = true;
          clearInterval(auth_timetimer);
        }
        }, 1000);
          var uri = "http://www.365think.cn:8080/api/v1/verifyCode?telephone=" + self.tel
          this.$http.get(uri)
          .then((data) => {
            if (data["body"]["code"] != "200") {
              alert("手机号错误")
            }
          })
       }
    },
    login: function () {
      const self = this
       if (!(/^1[2345789]\d{9}$/.test(self.tel))) {
         alert("请输入正确的手机号码")
       } else if (self.tel == "" || !self.tel) {
         alert("请输入手机号码")
       } else {
        var uri = "http://www.365think.cn:8080/api/v1/register?telephone=" + self.tel + "&verifyCode=" + self.code 
        this.$http.get(uri)
        .then((data) => {
          if (data["body"]["code"] == "200") {
            window.location.href = "http://www.365think.cn:8081/static/app-signed.apk"
          } else {
            alert("验证码错误")
          }
        })
      }
    },

    getAuthCode:function () {
      this.sendAuthCode = false;
      this.auth_time = 60;
      var auth_timetimer =  setInterval(()=>{
        this.auth_time--;
        if(this.auth_time<=0){
          this.sendAuthCode = true;
          clearInterval(auth_timetimer);
        }
      }, 1000);
    }

  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img {
  width: 100%;
  display: block;
}
.u-content {
  border: 0;
  margin: 0;
  padding: 0;
}
.u-login {
  position: relative;
  margin-top: -1px;
}
.login-form {
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  width: 68%;
  height: 78%;
  left: 16%;
  top: 11%;
}
.login-form-list {
  height: 25%;
  width: 100%;
  border-radius: 7px;
  display: flex;
  flex-direction: row;
  align-items: center;
  background: #fff;
}
.login-form-input {
  width: 90%;
  margin-left: 5%;
  height: 60%;
}
input {
  font-size: 15px;
  color: #4d94ff;
  width: 100%;
  height: 100%;
}
.login-form-yz {
    width: 50%;
    margin-left: 5%;
    margin-right: 5%;
    height: 60%;
}
.login-form-send {
    width: 35%;
    height: 60%;
    margin-right: 5%;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    background: #FF6329;
    font-size: 12px;
    border-radius: 4px;
    color: #fff;
    text-align: center;
}
.login-button {
    background: #FF6329;
    color: #fff;
    font-size: 20px;
    justify-content: center;
    box-shadow: 0px 6px 0px #FF4500;
}
</style>
