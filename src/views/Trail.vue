<template>
  <div>
    
    <van-row>
      <van-form>
          <h2 class="title">贷款试算工具</h2>
        <van-cell-group>
      <van-field v-model="amount" type="digit" label="借款金额(元)"></van-field>
      <van-field v-model="rate" type="digit" label="日息（万几）"></van-field>
      <van-field v-model="termNum" type="digit" label="期数"></van-field>
      <van-field readonly clickable :value="repayDay" label="还款日" @click="repayDayShow = true"></van-field>
      <van-field readonly clickable label="借款日" :value="requestFormatDate" @click="calendarShow = true"></van-field>
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
      <van-cell title="借款金额" >{{yuanFormat(traildata.Prin)}}</van-cell>
      <van-cell title="每月还款日">每月{{traildata.RepayDay}}日</van-cell>
      <van-cell title="总期数">{{traildata.TermNum}}期</van-cell>
      <van-cell title="还款金额" size="large" :value="traildata.Prin+traildata.Interest" >
        本金: {{yuanFormat(traildata.Prin)}} 利息: {{yuanFormat(traildata.Interest)}}
      </van-cell>
    </van-cell-group>
  </van-row>
  <van-row v-if="!loading">
    <h2 class="title">还款计划</h2>
    <van-steps direction="vertical" active="-1">
<van-step v-for="item in traildata.RepayPlans" v-bind:key="item.TermNum">
    <h3>还款金额：{{yuanFormat(item.Prin+item.Interest)}}</h3>
    <p>还款日:{{dateFormatter(new Date(item.RepayDate))}}</p>
    <p>本金:{{yuanFormat(item.Prin)}} 利息:{{yuanFormat(item.Interest)}}</p>
  </van-step>
</van-steps>
  </van-row>
  <van-popup v-model="calendarShow" position="bottom" :get-container="getContainer" >
  <van-datetime-picker
  @confirm="calendarConfirm"
  type="date"
  title="选择年月日"
  :min-date="minDate"
  :max-date="maxDate"
/>

  </van-popup>
  <van-popup v-model="repayDayShow" position="bottom" :get-container="getContainer" >
  <van-picker
  title="每月"
  show-toolbar
  :columns="columns"
  @confirm="onRepayConfirm"
/>
</van-popup>
  <!-- <van-calendar v-model="calendarShow" @confirm="calendarConfirm" /> -->
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
      traildata:null,
      amount:null,
      repayDay:null,
      paymentDate:null,
      requestFormatDate: null,
      requestDate:null,
      rate:null,
      termNum:null,
      loading:true,
      calendarShow:false,
      repayDayShow:false,
      minDate: new Date(2020, 0, 1),
      maxDate:new Date(2025, 10, 1),
      columns:[]
    }
  },
  created(){
    for(let i=1;i<=31;i++){
      this.columns.push(i+'日');
    }
  },
  methods:{
    trail(){
      this.loading = true;
      console.log(this.amount);
      var data = {
        "amount": this.amount*100,
        "rate": parseInt(this.rate)*100,
        "repayDay":parseInt(this.repayDay),
        "requestDate": this.requestDate.getTime(),
        "termNum": parseInt(this.termNum),
        "interestType": "EQUAL_LOAN"
      };
      axios.post("http://loanapi.zhonghcc.com/loan/trail",JSON.stringify(data)).then(resp=>{
        this.traildata = resp.data;
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
    calendarConfirm(value){
      console.log(value);
      // this.calendarConfirm=false;
      this.requestDate = value;
      this.requestFormatDate = this.dateFormatter(this.requestDate);
      this.calendarShow=false;
    },
    getContainer() {
      return document.querySelector('.app');
    },
    onRepayConfirm(value){
      console.log(value);
      this.repayDay = value;
      this.repayDayShow=false;
    },
    yuanFormat(v){
      return parseFloat((Number(v)/100).toFixed(2))+' 元';
    }
  },
  computed:{

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
