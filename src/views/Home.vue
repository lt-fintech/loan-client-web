<template>
  <div class="home">
    <van-row class="padding" style="margin-top:16px;">
    <van-button type="primary" icon="add-o" @click="goTrail" style="height:100px;font-size:26px;" block>还款计划试算</van-button>
    </van-row>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'Home',
  methods:{
    goTrail(){
      this.$router.push({name:'Trail'});
    }
  },
  components: {
  }
}
</script>
