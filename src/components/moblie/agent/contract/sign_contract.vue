<template>
  <div id="a_sign_contract">
    <cavans :contract="contract"  @aimgurl="commit"></cavans>
  </div>
</template>
<script>
export default {
  name: "goods_details",
  props:['contract'],
  data() {
    return {
      // imgurl: ""
    };
  },
  created() {
    console.log(this.$route.query);
    // console.log(this.$store.state.contract.body);
  },
  methods: {
    commit(data) {
      let timestamp = new Date().getTime();
      this.contract.agent_sign=data;
      this.contract.agent_signdate=timestamp;
      //  this.$emit("update:contract")
      this.$routerto('a_submit_contract',this.$route.query)
    },
  }
};
</script>
<style lang="scss">
#a_sign_contract {
  nav {
    position: relative;
    .van-icon-arrow-left {
      position: absolute;
      left: 0.6rem;
      top: 50%;
      transform: (translate(0, -50%));
    }
  }
}
// .van-dialog {
//   font-size: 0.3rem;
// }
// .van-dialog__message {
//   font-size: 0.3rem;
// }
// .van-button {
//   font-size: 0.3rem;
// }
</style>
<style lang="scss" scoped>
#a_sign_contract {
  width: 100%;
  nav {
    width: 100%;
    text-align: center;
    line-height: 1.5rem;
    height: 1.5rem;
    position: fixed;
    top: 0;
    font-size: 0.46rem;
    background: white;
    border-bottom: 0.1rem solid #b5b5b5;
  }
  main {
    margin-top: 1.5rem;
    padding: 0.5rem;
    background: #ffffff;
    div {
      width: 100%;
      background: #f2f2f2;
      height: 2rem;
      border-radius: 0.05rem;
      border: 0.01rem solid #8e8e8e;
    }
    footer {
      width: 100%;
      button {
        width: 100%;
        margin-top: 1rem;
        background: #00adef;
        color: white;
        height: 0.8rem;
      }
    }
  }
}
</style>
