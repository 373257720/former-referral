<template>
  <div id="a_submit_contract">
      <nav class="a_submit_contract">
        <van-icon name="arrow-left" @click="$router.go(-2)" />{{$t('agent.SignTheContract')}}
      </nav>
      <main>
        <article>
          <contractcomponent :contract="contract"></contractcomponent>
          <footer>
              <button @click="signproject4">{{$t('common.Submit')}}</button>
          </footer>
        </article>
      </main>
<!--      <mbottom></mbottom>-->

  </div>
</template>

<script>
export default {
  name: "goods_details",
  props:['contract'],
  data() {
    return {
      signId :'',
      projectId:'',
    };
  },
// beforeRouteEnter(to,from,next){
//       next((vm)=>{ //参数vm就是当前组件的实例。
//       console.log(from);

//         // console.log(to,from,vm.$route.query)
//         // if(from.name!="a_sign_contract" && vm.iswatch==2){
//         //   next({name: 'agent_set_contract',query:vm.$route.query});
//         // }
//       })
//   },
  // beforeRouteLeave(to,from,next){
  //     console.log(to,from)
  //       if(to.name=='a_submit_contract'){
  //          next({path: '/mysign'});
  //       }else{
  //         next()
  //       }
  // },
  created() {
    this.projectId=this.$route.query.projectId?this.$route.query.projectId:'';
    this.signId = this.$route.query.signId?this.$route.query.signId:-1;
  },
  methods: {
    // 签约
    signproject4() {
      for(let i in this.contract){
        if(this.contract[i]==''){
          this.$dialog
            .confirm({
              title: this.$t('ContractWrods.PleaseReturnToCompleteInformation')
              // message: "弹窗内容"
            })
            .then(() => {
              // this.$routerto('agent_set_contract',this.$route.query)
              this.$router.go(-2)
            });
          return;
        }
      };

      this.$loading();
        this.$global.post_encapsulation(`${this.$baseurl}/bsl_web/projectSign/signProject4`,
          {
            signId: this.signId,
            projectId:this.projectId,
            signAgreement: JSON.stringify(this.contract),
            X_Token:this.$store.state.X_Token,
          }
        )
        .then(res => {
        this.$toast.clear();
        if (res.data.resultCode == 10000) {
          // this.signId = res.data.data.signId;
          // this.token = res.data.data.visitToken;
          this.$dialog
            .alert({
              title: res.data.resultDesc,
              message: this.$t('ContractWrods.ConfirmAndUploadToBlockchain')
            })
            .then(() => {
               this.$routerto("uploadtoblock",  {
                  signId: this.signId,
                  projectId: this.projectId,
                  signStatus: 4
                });
            });
        } else {
          this.$dialog
            .alert({
              title: res.data.resultDesc,
            })
            .then(() => {
              this.$routerto('mysign');
            });
        }
      });
    },

    // iframe传值
    // handleMessage(event) {
    //   var data = event.data;
    //   switch (data.cmd) {
    //     case "returnFormJson":
    //       // 处理业务逻辑
    //       this.childData = data;
    //       console.log(this.childData);

    //       break;
    //   }
    // },
    // let p = new Promise((resolve, reject) => {
    //   this.iframeState = true;
    //   resolve("success");
    // });
    // p.then(result => {
    //   // console.log(result); //success
    //   if (result == "success") {
    //     let iframeWin = this.$refs.iframe.contentWindow;
    //     return iframeWin;
    //   }
    // }).then(iframeWin => {
    //   console.log(this.str);
    //   iframeWin.postMessage(
    //     {
    //       cmd: "toson",
    //       params: this.str
    //     },
    //     "*"
    //   );
    // });
  }
};
</script>
<style lang="scss">
#a_submit_contract {
    height: 100%;
    width: 100%;
  nav {
    position: relative;
    .van-icon-arrow-left {
      position: absolute;
      left: 0.6rem;
      top: 50%;
      transform: (translate(0, -50%));
    }
  }
.van-cell{
  padding: 0;
}
}

</style>
<style lang="scss" scoped>
 #a_submit_contract {
    width: 100%;
      height: 100%;
    padding: 1.5rem 0 1.3rem 0;
    nav.a_submit_contract{
      width: 100%;
      height: 100%;
      text-align: center;
      line-height: 1.5rem;
      height: 1.5rem;
      position: fixed;
      top: 0;
      z-index: 5;
      font-size: 0.46rem;
      background: white;
      border-bottom: 0.1rem solid #b5b5b5;
    }
    div.middle {
      /*margin: 0 0.5rem;*/
      box-sizing: border-box;
    }
    main {
      // margin-top: 1.5rem;
      // margin-bottom: 1.5rem;
      padding: 0.5rem;
      height: 100%;
      width: 100%;
      background: #ffffff;
      article{
      height: 100%;
      display:-webkit-box;
      display: -moz-box;
      display: -ms-flexbox;
      display: -webkit-flex;
      display: flex;
       flex-direction:column;
      -webkit-flex-direction:column;
      -webkit-justify-content:space-between;
      justify-content:space-between;
      width: 100%;
        >div{
           height: 85%;
        }
      footer {
        width: 100%;
        font-size: 0.42rem;
        display: flex;
        justify-content: center;
        button {
          background: #00adef;
          line-height: 1rem;
          color: white;
          height: 1rem;
          width: 8rem;
          border-radius:5px;
        }
        .blockchain {
          background: orange;
        }
      }

      }

    }
  }
</style>
