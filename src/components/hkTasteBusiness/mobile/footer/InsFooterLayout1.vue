<template>
 <div id="footer">
    <div class="footer-box">
          <div class="footertop fix">
            <div class="left">
              <img v-if="$Storage.get('locale') === 'E'" src="/images/pc/index_107eng.png" />
              <img v-else src="/images/pc/index_107.png" />
            </div>
            <div class="right">
              <div v-html="footerContact.Body"></div>
            </div>
          </div>
          <div class="footerNav">
            <ul>
                <li v-for="(item,index) in footerMenus" :key="index"  class="indexMeun">
                    <a href="javascript:;">{{item.Name}}<i class="downIcon" :class="{ 'downIcon': item.showSub, 'upIcon': !item.showSub }" @click="showMeun(item,index)"></i></a>
                    <ul v-if="item.Childs && item.Childs.length"  class="submeunMain" :class="'sub'+index">
                        <li v-for="(child,index2) in item.Childs" :key="index2">
                            <router-link :to="To(child)">{{child.Name}}</router-link>
                            <!-- <router-link  @click.native="closeSlideMenu(item.Childs)" :to="To(item.Childs)" slot="title">
                                <b>{{child.Name}}</b>
                            </router-link> -->
                        </li>
                    </ul>
                </li>
            </ul>
          </div>
          <!-- <div class="footerAccept" v-if="!isPtx">
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
          </div> -->

    </div>
    <div class="footerCpy">
      <p>Copyright © {{currentYear}} 天城實業（香港）有限公司 powered by
      <a href="https://eventizer.hk/" target="_blank">
        <img src="/images/mobile/footerlogo.png">
      </a>
      </p>
    </div>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';

@Component({
  components: {}
})
export default class InsFooter extends Vue {
  currentYear: number = 0;
  clickIndex: number = 0;
  footerMenus: any[] = [];
  footerContact: string = '';
  get isPtx () {
      if (localStorage.getItem('isPtx') === '0') {
        return false;
      } else {
        return true;
      }
  }
  // toUrl (n) {
  //   if (!n.IsNewWin && n.Url) {
  //     window.location.href = n.Url;
  //   } else if (n.IsNewWin && n.Url) {
  //     window.open(n.Url);
  //   }
  // }
  showMeun (item, index) {
    $('.sub' + index).slideToggle();
    this.clickIndex = index;
    item.showSub = !item.showSub;
    this.footerMenus.forEach((element, index) => {
      if (index !== this.clickIndex) {
        element.showSub = false;
      }
    });
  }
  closeSlideMenu (n) {
    this.$store.dispatch('isShowMenu', false);
  }
  To (n) {
    let url = '';
    if (n.Type === 0) {
      url = n.Url;
    } else if (n.Type === 1 && n.IsAnchor === false) {
      url = '/cms/catDetail/' + n.Value.Id;
    } else if (n.Type === 1 && n.IsAnchor === true) {
      url = '/CMS/catDetail/' + n.ParentId + '#' + n.Value.Id;
    } else if (n.Type === 2) {
      url = '/CMS/content/' + n.Value.Id;
    } else if (n.Type === 3) {
      url = '/RegNPay/Form/' + n.Value.Id;
    } else if (n.Type === 4 && !this.$store.state.catMenuType) {
      url = '/product/cat/' + n.Value.Id;
    } else if (n.Type === 4 && this.$store.state.catMenuType) {
      url =
        '/product/search/-?catalogs=' +
        JSON.stringify([parseInt(n.Value.Id)]) +
        '&type=0';
    } else if (n.Type === 5) {
      url =
        '/product/search/-?attrs=' +
        JSON.stringify([{ Id: parseInt(n.Value.Id), Vals: [] }]) +
        '&type=0';
    } else {
      url =
        '/product/search/-?attrs=' +
        JSON.stringify([
          { Id: parseInt(n.ParentId), Vals: [parseInt(n.Value.Id)] }
        ]) +
        '&type=0';
    }
    return url;
    // return n.Type === 1 ? '/cms/catDetail/' + n.Value.Id : n.Type === 2 ? '/CMS/content/' + n.Value.Id : n.Type === 3 ? '/RegNPay/Form/' + n.Value.Id : n.Type === 4 && !this.$store.state.catMenuType ? '/product/cat/' + n.Value.Id : n.Type === 4 && this.$store.state.catMenuType ? '/product/search/-?catalogs=' + JSON.stringify([parseInt(n.Value.Id)]) + '&type=0' : n.Type === 5 ? '/product/search/-?attrs=' + JSON.stringify([{ Id: parseInt(n.Value.Id), Vals: [] }]) + '&type=0' : '/product/search/-?attrs=' + JSON.stringify([{ Id: parseInt(n.ParentId), Vals: [parseInt(n.Value.Id)] }]) + '&type=0';
  }
  getMenu () {
    this.$Api.promotion.getMenu().then((result) => {
      this.footerMenus = result.ReturnValue.FooterMenus;
      // let menus = JSON.parse(JSON.stringify(result.ReturnValue.FooterMenus));
        result.ReturnValue.FooterMenus.forEach((element) => {
          element.showSub = false;
      });
    });
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
  @Watch('$route', { deep: true })
  onIdChange () {
    $('.submeunMain').hide();
    // let menus = JSON.parse(JSON.stringify(this.$store.state.footerMenus));
    this.$store.state.footerMenus.forEach((element) => {
      element.showSub = false;
    });
    this.footerMenus = this.$store.state.footerMenus;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.sub-comments-leave-active,
.sub-comments-enter-active {
  transition: max-height 0.3s linear;
}
.sub-comments-enter,
.sub-comments-leave-to {
  max-height: 0;
  // opacity: 0;
}
.sub-comments-enter-to,
.sub-comments-leave {
  max-height: 20rem;
}
.submeunMain{
  display: none;
}
.SubMeun0{
  display: none;
}
.SubMeun1{
  display: none;
}
#footer{
  width: 100%;
  background: url('/images/mobile/footerback.png') center center;
  background-size: contain;
  .footer-box{
    width: 90%;
    margin: 0 auto;
    padding-top: 2rem;
    padding-bottom: 0;

    .footerAccept{
      width: 100%;
      >p{
        font-size: 1.4rem;
        color:#fff;
        text-align: center;
        margin-bottom: 1rem;
      }

      >div {
        text-align: center;
        img {
          height: 2.5rem;
          margin: 0.3rem 0.5rem;
        }
      }
    }
    .footerNav{
      width: 100%;
      margin: 0 auto;
      margin-top: 3rem;
      margin-bottom: 0;
      >ul>li{
        width: 100%;
        display: block;
        // line-height: 4rem;
        text-align: center;
        margin-bottom: 1.5rem;
        position: relative;
        border-bottom: 1px solid #6c7880;
        padding-bottom: 1rem;
        &:last-child{
          border-bottom: none;
        }
        >ul{
          position: relative;
          padding-top: 3rem;
          margin-top: -3rem;
          z-index: 1;
          background-color: rgba(255, 255, 255, 0.1);
          margin-bottom: -1rem;
        }
        >ul>li{
        width: 100%;
        display: inline-block;
        background: transparent;
        border-radius: 10px;
        // height: 3.5rem;
        // line-height: 3.5rem;

        text-align: center;
        margin-bottom: .5rem;
          >a{
            font-size: 1.4rem;
            color:#c4d0d8;
            font-weight: 500;
            text-decoration: none;
          }
        }
        >a{
          font-size: 1.4rem;
          color:#f5f6f7;
          font-weight: 500;
          text-decoration: none;
          position: relative;
          display: flex;
          // background: #ffffff;
          // border-radius: 5px;
          margin-bottom: .5rem;
          z-index: 100;
          align-items: center;
          justify-content: center;
          margin-left: 2rem;
          i.downIcon{
              background: url('/Images/mobile/downicon.png') no-repeat center center;
              background-size: contain;
              width: 1rem;
              height: 1rem;
              display: block;
              transition: all 0.3s;
              transform: rotate(180deg);
              margin-left: 1rem;
              //  position: absolute;
              //  right: 1rem;
              //  top: 1.4rem;
          }
          i.upIcon {
          transform: rotate(360deg);
          transition: all 0.3s;
        }
        }
      }
    }
    .contactBox{
    width: 100%;
    display: inline-block;
    margin-top: 2rem;
      .w50{
        width: 50%;
        float: left;
        >p:nth-child(1){
          font-size: 1.4rem;
          text-align: center;
          color: #FFF;
          padding-bottom: .5rem;
        }
        >p:nth-child(2){
            font-size:2.2rem;
            text-align: center;
            color: #FFF;
        }
      }
    }
    >p{
      width:70%;
      margin: 0 auto;
      img{
        width: 100%;
      }
    }
    .footertop{
      .left{
        float: left;
        width: 34%;
        img{
          width: 90%;
          display: block;
        }
      }
      .right{
        float: left;
        width: 66%;
        /deep/ p{
          color: #c4d0d8;
          font-size: 1.2rem;
        }
      }
    }
  }
  .footerCpy{
      width: 100%;
      display: block;
      margin-top: 0;
      background-color: #1b2933;
      padding: 1rem 0;
      >p{
        color:#c4d0d8;
        font-size: 1rem;
        text-align: center;
        img{
          height: 2rem;
          display: inline-block;
          margin-left: 1rem;
          vertical-align: middle;
        }
      }
    }
}
</style>
