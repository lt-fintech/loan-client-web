<template>
  <div>
    <van-row>
      <van-form>
          <h2 class="title">贷款试算工具</h2>
        <van-cell-group>
      <van-field v-model="amount" type="digit" label="借款金额"></van-field>
      <van-field v-model="repayDay" type="digit" label="还款日"></van-field>
      <van-cell title="借款日" :value="requestFormatDate" label="借款日" @click="calendarShow = true"></van-cell>
        </van-cell-group>
      </van-form>
    </van-row>
  <van-row class="padding">
    <van-cell-group>
    <van-button type="primary" class="submit" block @click="trail">贷款试算</van-button>
    </van-cell-group>
  </van-row>
  <van-row v-if="!loading">
    <van-cell-group>
      <van-cell title="借款金额" :value="amount"></van-cell>
      <van-cell title="每月还款日" :value="trialdata.RepayDay"></van-cell>
      <van-cell title="总期数" :value="trialdata.TermNum"></van-cell>
      <van-cell title="还款金额" size="large" :value="trialdata.Prin+trialdata.Interest" :label="'本金:'+trialdata.Prin+' 利息:'+trialdata.Interest"></van-cell>
    </van-cell-group>
  </van-row>
  <van-row v-if="!loading">
    <h2 class="title">还款计划</h2>
    <van-steps direction="vertical" active="-1">
<van-step v-for="item in trialdata.RepayPlans" v-bind:key="item.TermNum">
    <h3>还款金额：{{item.Prin+item.Interest}}</h3>
    <p>还款日:{{dateFormatter(new Date(item.RepayDate))}}</p>
    <p>本金:{{item.Prin}} 利息:{{item.Interest}}</p>
  </van-step>
</van-steps>
  </van-row>
  <van-calendar v-model="calendarShow" @confirm="calendarConfirm" />
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
      trialdata:null,
      amount:null,
      repayDay:null,
      paymentDate:null,
      requestFormatDate: null,
      requestDate:null,
      loading:true,
      calendarShow:false
    }
  },
  created(){
    
  },
  methods:{
    trail(){
      this.loading = true;
      console.log(this.amount);
      var data = {
        "amount": 500,
        "rate": 500,
        "requestFormatDate": null,
        "requestDate":null,
        "repayDay": 17,
        "termNum": 6,
        "interestType": "EQUAL_LOAN"
      };
      axios.post("http://loanapi.zhonghcc.com/loan/trail",JSON.stringify(data)).then(resp=>{
        this.trialdata = resp.data;
        this.loading = false;
      }).catch(err=>{
        console.log(err);
        Notify({ type: 'warning', message: '运行失败，请联系客服处理' });
      });
    },
    dateFormatter (date) {
      const month = (date.getMonth() + 1).toString().padStart(2, '0');
      const strDate = date.getDate().toString().padStart(2, '0');
      return `${date.getFullYear()}-${month}-${strDate}`;
    },
    calendarConfirm(date){
      this.calendarConfirm=false;
      this.requestDate = date;
      this.requestFormatDate = this.dateFormatter(date);
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
