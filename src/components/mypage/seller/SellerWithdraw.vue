<template>
  <div>
    <div class="title">
      <!-- 타이틀 박스 -->
    </div>
    <div class="content">
      <div>
        <div class="field">
          <label for="idbox">아이디</label>
          <input type="text" id="idbox" placeholder="ID" v-model="sellerId">
        </div>
        <div class="field">
          <label for="pwbox">비밀번호</label>
          <input type="text" id="pwbox" placeholder="PASSWORD" v-model="sellerPw">
        </div>
      </div>
    </div>
    <div class="btnbox">
      <button class="btn submitBtn" @click="withdraw">회원탈퇴</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { mapActions } from 'vuex';

export default {
  data() {
    return {
      token: sessionStorage.getItem('token'),
      sellerId: '',
      sellerPw: '',
    };
  },
  methods: {
    ...mapActions(['changeisLoginAct']),
    async withdraw() {
      const url = '/ROOT/seller/delete';
      const headers = { 'Content-Type': 'application/json', token: this.token };
      const body = {sellerId: this.sellerId, sellerPw: this.sellerPw};
      const res = await axios.delete(url, { headers: { 'Content-Type': 'application/json', token: this.token }, data: {sellerId: this.sellerId, sellerPw: this.sellerPw}});
      console.log(res);
      if(res.data.status == 200) {
        alert('탈퇴완료하였습니다.');
        this.changeisLoginAct(false);
        sessionStorage.removeItem('token');
        this.$router.push('/');
      }
    }
  },
};
</script>

<style lang='scss' scoped>
.title {
    height: 75px;
    // background: #eee;
  }
  .content {
    margin-top: 15px;
  }
  .field {
    label {
      float: left;
      width: 120px;
      height: 46px;
      padding: 10px 15px 14px 15px;
      text-align: center;
      font-size: 18px;
      color: #222;
      border: 1px solid #ccc;
      border-right: none;
      background: #E2E2E2;
    }
    margin-bottom: 10px;
    input[type='text'] {
      display: block;
      float: left;
      width: 608px;
      height: 46px;
      padding: 13px 16px;
      border: 1px solid #ccc;
      font-size: 15px;
      font-weight: normal;
      line-height: 20px;
      color: #111;
      letter-spacing: 0;
      &::placeholder {
        color: #999;
      }
    }
    &:after {
      display: block;
      content: "";
      clear: both;
    }
  }
  .btn {
    width: 150px;
    padding: 10px 20px;
    font-size: 20px;
    color: #222;
    border: 3px solid #222;
    background: white;
    font-weight: bolder;
    &:hover {
      background: #222;
      border: 3px solid white;
      color: #e2e2e2;
      transition: all 0.5s;
      cursor: pointer;
    }
  }
  .btnbox {
    width: 730px;
    &::after {
      display: block;
      content: '';
      clear: both;
    }
  }
  .submitBtn {
    float: right;
  }
</style>
