<template>
  <div>
    <van-row>
      <van-form>
          <h2 class="title">贷款试算工具</h2>
        <van-cell-group>
      <van-field v-model="amount" type="digit" label="借款金额"></van-field>
      <van-field v-model="repayDay" type="digit" label="还款日"></van-field>
      <van-field v-model="paymentDate" type="digit" label="借款日"></van-field>
        </van-cell-group>
      </van-form>
    </van-row>
  <van-row class="padding">
    <van-cell-group>
    <van-button type="primary" class="submit" block @click="trail">贷款试算</van-button>
    </van-cell-group>
  </van-row>
  <van-row>
    {{repayplan}}
  </van-row>
  </div>
</template>

<script>
import axios from 'axios'
import {Notify} from 'vant'
export default {
  name: 'Trail',
  props:[],
  data(){
    return {
      repayplan:{},
      amount:null,
      repayDay:null,
      paymentDate:null,
    }
  },
  created(){
    
  },
  methods:{
    trail(){
      console.log(this.amount);
      var data = {
        "amount": 500,
        "rate": 500,
        "requestTime": 1597640186380,
        "repayDay": 17,
        "termNum": 6,
        "interestType": "EQUAL_LOAN"
      };
      axios.post("http://loanapi.zhonghcc.com/loan/trail",JSON.stringify(data)).then(resp=>{
        this.repayplan = resp.data;
      }).catch(err=>{
        console.log(err);
        Notify({ type: 'warning', message: '运行失败，请联系客服处理' });
      });
    }
  },
  components: {
  }
}
</script>
<style scoped>
.submit{
  margin-top:16px;
  margin-bottom:16px;
}
</style>
