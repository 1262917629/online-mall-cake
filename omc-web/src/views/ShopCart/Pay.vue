<template>
  <div class="pay">
    <h2>确认收货信息</h2>
    <ul class="detail">
      <li>
        <label for>收货人</label>
        <input type="text" v-model="userName" />
      </li>
      <li>
        <label for>收货电话</label>
        <input type="text" v-model="userPhone" />
      </li>
      <li>
        <label for>收获地址</label>
        <input type="text" v-model="address" />
      </li>
    </ul>
    <div class="pay-way">
      <h2>选择支付方式</h2>
      <span class="price">支付金额:{{totalPrice}}</span>
      <div>
        <img src="../../assets/images/vx.jpg" alt />
        <img src="../../assets/images/zfb.jpg" alt />
      </div>
      <div class="button" @click="submitOrder">确认订单</div>
    </div>
  </div>
</template>

<script >
import { defineComponent, reactive, toRefs } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { reqSubmitOrder } from '@/api/index.js'
export default defineComponent({
  setup() {
    const state = reactive({
      shopCartId: '',
      userPhone: '',
      userName: '',
      address: '',
      totalPrice: 0
    })
    const route = useRoute()
    const router = useRouter()
    state.shopCartId = route.params.shopCartId
    state.totalPrice = route.params.totalPrice
    // 确认订单
    function submitOrder() {
      if (state.userName == '' || state.userPhone == '' || state.address == '') {
        return alert('必选项不能为空')
      }
      const data = {
        id: state.shopCartId,
        payType: 1,
        total: state.totalPrice,
        amount: 2,
        name: state.userName,
        phone: state.userPhone,
        address: state.address
      }
      if (confirm('是否支付')) {
        reqSubmitOrder(data).then(res => {
          if (res.data.response) {
            alert('支付成功')
            router.push({ name: 'desk' })
          }
        })
      }
    }
    return {
      ...toRefs(state),
      submitOrder
    }
  }
})
</script>

<style scoped lang="less">
.pay {
  color: @fontColor;
  width: 1200px;
  margin: 20px auto;
  text-align: center;
  .detail {
    width: 500px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    li {
      width: 500px;
      display: flex;
      margin-top: 20px;
      label {
        flex: 1;
        text-align: center;
        color: @fontColor;
        font-size: 20px;
        font-weight: 600;
        margin-right: 20px;
      }
      input {
        outline: none;
        width: 300px;
      }
    }
  }
  .pay-way {
    h2 {
      margin-bottom: 20px;
    }
    .price {
      margin-top: 20px;
      color: red;
    }
    div {
      margin-top: 40px;
      display: flex;
      justify-content: center;
      img {
        width: 400px;
      }
    }
  }
  .button {
    margin: 10px auto;
    height: 40px;
    line-height: 40px;
    width: 140px;
    background-color: @fontColor;
    color: #fff;
    cursor: pointer;
  }
}
</style>
