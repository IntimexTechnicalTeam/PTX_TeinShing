<template>
<div>
    <!-- 正常购买模式 -->
    <div class="memberlogin">
        <div class="menberCentral" @click="menberCentral">
            <img class="showMenberCentral" src="/images/mobile/ptx_12.png">
            <transition name="slide-fade">
              <div class="lang_flow" v-show="showMenberCentral" @click="memberCentral">
                <i class="Membershipup"></i>
                <div class="title">
                  <span>{{ $t("Account.Welcome") }} {{ MemberName }}</span>
                </div>
                <router-link to="/account/memberInfo">{{$t('Account.MemberInformation')}}</router-link>
                <router-link to="/account/notification">{{$t('Account.MyMessages')}}</router-link>
                <router-link to="/order/List">{{$t('Account.MyOrder')}}</router-link>
                <router-link to="/account/myFavorite">{{$t('Account.MyFavorite')}}</router-link>
                <router-link to="/account/deliveryAddress">{{$t('Account.DeliveryAddress')}}</router-link>
                <router-link to="/account/mycoupon">{{$t('Account.MemberInformation')}}</router-link>
                <router-link to="/account/message">{{$t('Account.LatestNews')}}</router-link>
                <router-link to="/account/ptxorder">{{$t('Enquiry.PTXOrder')}}</router-link>

                  <!-- <div data-to="/account/memberInfo" class="ii">{{$t('Account.MemberInformation')}}</div> -->
                  <!-- <div data-to="/account/notification" class="ii">{{$t('Account.MyMessages')}}</div> -->
                  <!-- <div data-to="/order/List" class="ii">{{$t('Account.MyOrder')}}</div> -->
                  <!-- <div data-to="/account/myFavorite" class="ii">{{$t('Account.MyFavorite')}}</div> -->
                  <!-- <div data-to="/account/deliveryAddress" class="ii">{{$t('Account.DeliveryAddress')}}</div> -->
                  <!-- <div data-to="/account/mycoupon" class="ii">{{$t('MyCoupon.MyCoupon')}}</div> -->
                  <!-- <div data-to="/account/message" class="ii">{{$t('Account.LatestNews')}}</div> -->
                  <!-- <div data-to="/account/ptxorder" class="ii">{{$t('Enquiry.PTXOrder')}}</div> -->
                  <div class="logout" @click="logout">{{$t('Account.Logout')}}</div>
              </div>
            </transition>
        </div>
    </div>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import Cookie from 'js-cookie';
import Auth from '@/sdk/common/Auth';
import sdk from '@/sdk/InstoreSdk';
@Component
export default class InsLangSwitch extends Vue {
  private showMenberCentral:boolean = false;
  MemberName:string='';
  get isPtx () {
      if (localStorage.getItem('isPtx') === '0') {
        return false;
      } else {
        return true;
      }
  }
  menberCentral () {
    if (!this.$Storage.get('isLogin')) {
      window.location.href = '/account/login';
    } else {
      this.showMenberCentral = !this.showMenberCentral;
    }
  }
  memberCentral (e) {
    let target = e.target as HTMLElement;
    if (target.className === 'ii' && target.dataset.to) {
      this.$router.push({
        path: target.dataset.to
      });
    }
  }
  logout () {
    this.$Api.member.logout().then((result) => {
      if (result) this.$message('Message.SucceedInOperating');
      this.$router.push('/');
      this.Reload();
    });
  }
  // 获取会员信息
   getMemberInfo () {
     let _this = this;
     sdk.api.member.getProfile().then(
       function (data) {
         console.log(data, '获取会员信息');
         if (data !== null) {
           _this.MemberName = data.FirstName + '  ' + data.LastName;
         }
       }
     );
   }
  get user() {
    return this.$store.state.user;
  }
  mounted() {
    // console.log(this.$store.state.user, 'useruser');
    this.getMemberInfo();
  }
}
</script>
<style scoped lang="less">
  .menberCentral{
    float: left;
    // height: 3.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
    .lang_flow{
    position: absolute;
    top: 5rem;
    left: 50%;
    transform: translateX(-50%);
    width: 96%;
    background: #ffffff;
    z-index: 999;
    box-shadow: 0 0 5px #afadad;
    padding: 1rem;
    box-sizing: border-box;
        border-radius: 3px;
    .Membershipup{
      width: 17px;
      height: 12px;
      background: url('/images/mobile/Membershipup.png') no-repeat;
      position: absolute;
          right: 0.6rem;
    top: -12px;
    }
    .title{
      width: 100%;
      height: 4rem;
      border-radius: 3px;
      background-color: #2f4858;
      text-align: center;
      margin-bottom: 1rem;
      display: flex;
      justify-content: center;
      align-items: center;
      span{
        font-size: 1.4rem;
        color: #fff;
      }
    }
      >a{
        color:#808080;
        font-size: 1.3rem;
        height: 4rem;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: #f5f5f5;
        margin-bottom: 1rem;
        border-radius: 3px;
        overflow: hidden;
        position: relative;
        width: 100%;
        &.router-link-active{
          color: #de2910;
          &::before{
            content: '';
            width: 6px;
            height: 100%;
            position: absolute;
            left: 0;
            top: 0;
            background-color: #de2910;
          }
        }
      }
      .logout{
        color:#fff;
        font-size: 1.3rem;
        height: 4rem;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: #de2910;
        border-radius: 3px;
      }
    }
    img{
      width:2rem;
      margin: 0 auto;
      display: block;
    }
  }
/* 可以设置不同的进入和离开动画 */
/* 设置持续时间和动画函数 */
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active for below version 2.1.8 */ {
  transform: translateY(-10px);
  opacity: 0;
}
</style>
