<template>
  <div id="login">
    <h2>
      <img src="../../assets/indexLogo.png" alt />
      <button class="switchLan" @click="switchLan(lan)">{{lantext}}</button>
    </h2>
    <div class="main">
      <p>{{remind}}</p>
      <!-- <van-form> -->
      <!--      <div class="username common" v-if="$route.query.email">-->
      <!--        <van-field v-model.trim="username" placeholder="电子邮箱" disabled />-->
      <!--      </div>-->
      <!--      v-if="!$route.query.email"-->
      <van-form>
        <div class="username common">
          <van-field v-model.trim="username" :placeholder="$t('common.Email')" clearable />
        </div>
        <div class="password common">
          <van-field
            v-model.trim="password"
            type="password"
            :placeholder="$t('common.PassWord')"
            clearable
          />
        </div>
        <div class="loginbtn common">
          <button @click="login">{{$t('common.LogIn')}}</button>
        </div>
      </van-form>
      <div class="registerbtn common">
        <button @click="$routerto('register',{email:username})">{{$t('common.Register')}}</button>
      </div>
      <div class="tologin">
        <p class="tologin" @click="$routerto('forgotpassword')">{{$t('common.forgetpassword')}}</p>
      </div>
    </div>
  </div>
</template>
<script>
// import { i18n } from "../../language";

export default {
  name: "login",
  // inject: ["reload"],
  data() {
    return {
      username: "",
      password: "",
      remind: "",
      lan: ""
    };
  },
  computed: {
    lantext: function() {
      if (this.lan == "en_US") {
        return "En";
      } else if (this.lan == "zh_CN") {
        return "中文";
      }
    }
  },
  created() {
    if (this.$route.query.email) {
      this.username = this.$route.query.email ? this.$route.query.email : "";
    }
    this.lan = this.$i18n.locale;
    this.switchLan();
  },
  // beforeRouteLeave(to, from, next) {
  //   // console.log(to, from)
  //   if (to.name == 'i_emailto_confirm') {
  //     next(false);
  //   } else {
  //     next()
  //   }
  // },
  methods: {
    switchLan(lan) {
      // console.log(lan);
      let language = this.lan;
      if (lan == "en_US") {
        language = "zh_CN";
      } else if (lan == "zh_CN") {
        language = "en_US";
      }
      this.$global
        .get_encapsulation(`${this.$baseurl}/bsl_web/base/language.do`, {
          lan: language
        })
        .then(res => {
          if (res.data.resultCode === 10000) {
            // console.log(this.$i18n.locale);
            this.lan = language;
            localStorage.setItem("language", language);
            this.$Local(language);
            this.$i18n.locale = language;
            this.$store.dispatch(
              "X_Token_actions",
              JSON.parse(res.data.data).X_Token
            );
          } else {
            this.$toast(res.data.resultDesc);
          }
        });
    },
    login() {
      this.remind = "";
      if (this.username && this.password) {
        this.$loading();
        this.$global
          .post_encapsulation(`${this.$baseurl}/bsl_web/user/login.do`, {
            bslEmail: this.username,
            bslPwd: this.password
          })
          .then(res => {
            this.$toast.clear();
            var rescode = res.data.resultCode;
            if (rescode == 10000) {
              window.sessionStorage.clear();
              // window.sessionStorage.setItem('Xoken',res.data.data.X_Token)
              this.$store.dispatch("reset_actions", this.$restore_obj);
              this.$store.dispatch("X_Token_actions", res.data.data.X_Token);
              this.$store.dispatch("usertype", res.data.data.userType);
              this.$store.dispatch("setUser", this.username);
              // this.reload();
              if (res.data.data.isAuth == 1) {
                this.$routerto("mhome");
              } else if (res.data.data.isAuth == 0) {
                this.$routerto("usercheck");
              }
            }
            this.remind = res.data.resultDesc;
          })
          .catch(err => {});
      } else {
        this.remind = this.$t("common.AccountAndPasswordCannotBeEmpty");
      }
    }
  }
};
// 10011	登录账号不能为空
// 10012	密码不能为空
// 10013	账号不存在
// 10014	账号或密码不正确
// 10000	登录成功
</script>
<style lang='scss'>
#login {
  .van-field__body {
    //  width: 100%;
    height: 1rem;
    border: 1px solid #dddddd;
    border-radius: 3px;
    background: #f6f6f6;
    padding: 0.1rem 0.3rem;
    box-sizing: border-box;
  }

  .van-field__control {
    font-size: 0.42rem;
    // line-height: 0.7rem;
  }

  .van-field__clear {
    // height: 0.1rem;
    font-size: 0.38rem;
  }

  .username,
  .password {
    .van-field {
      padding: 0;
      width: 9.8rem;
    }
  }
}
</style>
<style lang='scss' scoped>
#login {
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
  // align-items: center;
  // align-items: center;
  // justify-content: center;

  h2 {
    height: 40%;
    position: relative;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    button.switchLan {
      position: absolute;
      padding: 0 12px;
      color: #fff;
      top: 0.5rem;
      right: 0.8rem;
      background: rgb(194, 192, 192);
      font-size: 0.38rem;
      line-height: 0.68rem;
      text-align: center;
      border-radius: 20px;
      cursor: pointer;
    }

    img {
      width: 4.8rem;
      height: 2.6rem;
      /*max-height: 100%;*/
      /*max-width: 100%;*/
    }
  }

  @media screen and (min-width: 1024px) {
    h2 {
      /* height: 50%; */
    }
  }

  .main {
    /*flex: 1;*/
    /* // width: 100%; */
    display: flex;
    flex-direction: column;
    align-items: center;

    p {
      font-size: 0.38rem;
      height: 0.84rem;
      line-height: 0.84rem;
      color: #f36c69;
    }

    div.tologin {
      width: 9.8rem;
      display: flex;
      justify-content: flex-end;

      p {
        //  align-self:  flex-end;
        color: #00adef;
        text-decoration: underline;
        cursor: pointer;
      }
    }

    div.common {
      margin-bottom: 0.5rem;
    }

    button {
      /*margin-bottom: 0.5rem;*/
      color: white;
      border-radius: 5px;
      width: 9.8rem;
      line-height: 1rem;
      height: 1rem;
      font-size: 0.42rem;
    }

    .loginbtn button {
      background: #00adef;
    }

    .registerbtn button {
      background: #ff7c2c;
    }
  }
}
</style>
