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
  <van-row>
    <van-cell-group>
    <van-button type="primary" class="submit" block>贷款试算</van-button>
    </van-cell-group>
  </van-row>
  </div>
</template>

<script>

export default {
  name: 'Trail',
  components: {
  }
}
</script>
<style scoped>
.submit{
  padding:0 32px;
  margin:16px;
}
</style>
