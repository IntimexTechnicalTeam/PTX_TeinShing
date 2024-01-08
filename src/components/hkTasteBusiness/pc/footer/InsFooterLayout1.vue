<template>
<div id="footer">
  <div class="footbg">
    <div class="footerMain">
        <!-- <div class="footerTop">
            <p><span>whatsapp&nbsp;{{$t('home.Order')}}</span><b>6289 1789</b></p>
            <p><span>{{$t('home.TelSearch')}}</span><b>6289 1789</b></p>
        </div> -->
        <div class="footerBotttom">
          <div class="footerLeft">
            <p><img src="/images/pc/index_107.png" /></p>

          </div>
          <div class="footerMiddle">
            <ul v-for="(n,index) in footerMenus" :key="index">
                <li>
                    <a href="javascript:;" v-if="n.Type === 0" @click="toUrl(n)"><span>{{n.Name}}</span></a>
                    <router-link :to="To(n)"  v-else><span>{{n.Name}}</span></router-link>
                  <ul>
                    <li v-for="(c,index2) in n.Childs" :key="index2">
                       <a href="javascript:;" v-if="c.Type === 0" @click="toUrl(c)">
                              {{c.Name}}
                        </a>
                       <router-link :to="To(c)" v-else>{{c.Name}}</router-link>
                    </li>
                  </ul>
                </li>
             </ul>
          </div>
          <div class="footerRight">
            <div class="title">
              {{$t("home.footercontact")}}
            </div>
            <div class="body" v-html="footerContact.Body"></div>
          </div>
          <div class="clear"></div>

        </div>
    </div>
    <p class="footercopy">
             <span>Copyright © {{currentYear}} 天城實業（香港）有限公司. Powered by
               <a href="https://eventizer.hk/" target="_blank">
               <img src="/images/pc/footerlogo.png">
               </a>
              </span>
             <!-- <span v-if="!isPtx">
                <p>{{$t('home.Weaccept')}}</p>
                <div>
                  <img src="/images/payment/stripe.png" />
                  <img src="/images/payment/WeChatPay.png" />
                  <img src="/images/payment/Alipay.png" />
                  <img src="/images/payment/PayMe.png" />
                  <img src="/images/payment/Paypal.png" />
                  <img src="/images/payment/MasterCard.png" />
                  <img src="/images/payment/VISA.png" />
                </div>
              </span> -->
            </p>
  </div>
</div>

</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class InsFooterLayout1 extends Vue {
  currentYear: number = 0;
  footerMenus: any[] = [];
  footerContact: string = '';
    get isPtx () {
      if (localStorage.getItem('isPtx') === '0') {
        return false;
      } else {
        return true;
      }
  }
  goToTop () {
    let sTop = document.documentElement.scrollTop;
    let times = setInterval(() => {
      sTop -= 50;
      if (sTop <= 0) {
        document.documentElement.scrollTop = 0;
        clearInterval(times);
      } else {
        document.documentElement.scrollTop = sTop;
      }
    }, 1);
  }
  toUrl (n) {
    if (!n.IsNewWin && n.Url) {
      window.location.href = n.Url;
    } else if (n.IsNewWin && n.Url) {
      window.open(n.Url);
    }
  }
  To (n) {
    return n.Type === 1 ? '/cms/catDetail/' + n.Value.Id : n.Type === 2 ? '/CMS/content/' + n.Value.Id : n.Type === 3 ? '/RegNPay/Form/' + n.Value.Id : n.Type === 4 && !this.$store.state.catMenuType ? '/product/cat/' + n.Value.Id : n.Type === 4 && this.$store.state.catMenuType ? '/product/search/-?catalogs=' + JSON.stringify([parseInt(n.Value.Id)]) + '&type=0' : n.Type === 5 ? '/product/search/-?attrs=' + JSON.stringify([{ Id: parseInt(n.Value.Id), Vals: [] }]) + '&type=0' : '/product/search/-?attrs=' + JSON.stringify([{ Id: parseInt(n.ParentId), Vals: [parseInt(n.Value.Id)] }]) + '&type=0';
  }
  getMenu () {
    this.$Api.promotion.getMenu().then((result) => {
      this.footerMenus = result.ReturnValue.FooterMenus;
    });
  }
  get currentlang () {
    return this.$i18n.locale;
  }
  getFooterContact() {
    this.$Api.cms
      .getContentByDevice({ key: 'footer_contact', IsMobile: false })
      .then((result) => {
        this.footerContact = result.CMS;
      });
  }
  mounted() {
    this.getFooterContact();
  }
  created () {
    var date = new Date();
    this.currentYear = date.getFullYear();
    this.getMenu();
  }
}
</script>

<style scoped lang="less">
/* 底部文件 */
.footbg{
    background: url('/images/pc/1-index_32.png') no-repeat center bottom;
    background-size: cover;
    width: 100%;
    display: inline-block;
    // padding-bottom: 10px;
    // min-height: 278px;
    // background-color: #4d4d4d;
}
.footerMain{
    width: 1200px;
    margin: 30px auto 0;
}
.footerTop{
    text-align: center;
    padding-top: 25px;
    padding-bottom: 25px;
    width: 100%;
}
.footerTop p{
    text-align: center;
    display: inline-block;
    margin-right: 50px;
}
.footerTop p span{
    font-size: 14px;
    color: #FFF;
    line-height: 35px;
    margin-right: 15px;
}
.footerTop p b{
    font-weight: 100;
    font-size: 35px;
    color: #FFF;
    line-height: 35px;
}
.footerBotttom{
    width: 100%;
    display: flex;
    justify-content: space-between;
}
.footerLeft{
    // float: left;
    width: 152px
}

.footerLeft p{
    width: 100%;
    display: block;
    font-size: 14px;
    color: #fff;
    padding-top: 20px;
}
.footerLeft p img{
    display: block;
    vertical-align: middle;
    // padding-left: 10px;
}
.footerMiddle{
  width: 680px;
  // text-align: center;
  >ul{
    >li{
      float: left;
      a{
        padding: 0 40px;
        font-size: 18px;
        color: #fff;
        margin-bottom: 20px;
        display: block;
      }
      >ul{
        li{
          a{
            color: #c4d0d8;
            font-size: 16px;
            margin-bottom: 10px;
            display: block;
          }
        }
      }
    }
  }
}
.footerRight{
    // float: right;
    width: 364px;
    text-align: left;
    .title{
      font-size: 18px;
      color: #fff;
      margin-bottom: 20px;
    }
    .body{
    /deep/ p{
      font-size: 16px;
      color: #c4d0d8;
      margin-bottom: 10px;
    }
    }
}

.footercopy{
  width: 100%;
  display: inline-block;
  margin-top: 20px;
  text-align: center;
  background-color: #1b2933;
  padding: 8px 0;
}
.footercopy span:nth-child(1){
  // float: left;
  color:#7a848c;
  font-size: 14px;
}
.footercopy span:nth-child(1) img{
  display: inline-block;
  vertical-align:middle;
  padding-left: 10px;
  height: 35px;
}
.footercopy span:nth-child(2){
  float: right;
  width: 40%;
  // text-align: center;
  color:#FFF;
  font-size: 14px;
  display: flex;

  >p{
    flex-shrink: 0;
    margin-top: 10px;
    margin-right: 10px;
  }
}
.footercopy span:nth-child(2) img{
  display: inline-block;
  vertical-align:middle;
  margin: 5px;
  height: 32px;
}
</style>
