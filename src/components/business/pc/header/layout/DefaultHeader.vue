<template>
<div class="header-layout"  v-cloak>
  <div class="headerBg" :class="{'ENG':$Storage.get('locale') === 'E'}">
      <div class="headerTop">
          <div class="inner fix">
            <div class="left">
              <p>
                {{$t("home.welcome")}}
              </p>
            </div>
            <div class="right">
              <!-- 搜索框开始 -->
              <div class="search-box">
                <el-select v-model="searchType" placeholder="please select">
                  <el-option
                    v-for="item in typeList"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                  </el-option>
                </el-select>

                <div class="search-input">
                  <input type="text" v-model="key" @keyup.enter="search" />
                  <span class="searchBtn" @click="search"></span>
                </div>
              </div>
              <!--搜索框结束  -->
              <!-- 我的喜爱开始 -->
              <div class="cartTop">
                  <router-link to="/account/MyFavorite">
                          <i class="handle-icon fav-icon"></i>
                  </router-link>
              </div>
              <!-- 我的喜爱结束 -->
              <!-- 会员登陆开始 -->
              <InsLogin class="memberLogin"></InsLogin>
              <!-- <div class="cartTop" >
                  <router-link to="/account/GetEnquiry">
                        <i class="handle-icon ptxicon"></i>
                  </router-link>
              </div> -->

              <!-- 购物车开始 -->
              <!-- <Shopcart class="memberLogin"></Shopcart> -->
              <!-- 购物车结束 -->
              <!-- 切换语言开始 -->
              <!-- <CodeSelect  /> -->
              <div class="langBox">
                  <InsLangSwitch></InsLangSwitch>
              </div>
              <!-- 切换语言结束 -->
            </div>
          </div>
      </div>
      <div class="headerBottom">
        <div class="inner fix">
          <!-- logo开始 -->
          <div class="logoBox">
            <a href="/" v-if="$Storage.get('locale') === 'E'"><img src="/images/pc/pcindex_09eng.png"></a>
            <a href="/" v-else><img src="/images/pc/pcindex_09.png"></a>

          </div>
          <!-- logo结束 -->
          <!-- 导航栏开始 -->
          <Menu />
          <!-- 导航栏结束 -->
        </div>

      </div>

  </div>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
@Component({
  components: {
    Menu: () =>
      import('@/components/business/pc/header/InsMenu.vue'),
    Shopcart: () =>
      import('@/components/business/pc/header/InsShoppingCart.vue'),
    InsLogin: () =>
      import('@/components/business/pc/header/InsLogin.vue'),
    InsLangSwitch: () =>
      import('@/components/business/pc/header/InsLangSwitch.vue'),
    CodeSelect: () =>
      import('@/components/business/pc/header/InsCodeSelect.vue')
  }
})
export default class DefaultHeader extends Vue {
  @Prop() private showInFixed!: boolean;

  private key: string = '';

  private typeList: any[] = [{
    value: 0,
    label: this.$t('product.Productname') + ''
  }, {
    value: 1,
    label: this.$t('product.ProductCMS') + ''
  }];

  private searchType: number = 0;
  get isPtx () {
      if (localStorage.getItem('isPtx') === '0') {
        return false;
      } else {
        return true;
      }
  }
  getMenu () {
    this.$Api.promotion
      .getMenu()
      .then(result => {
        this.$store.dispatch('setHeaderMenus', result.ReturnValue.HeaderMenus);
        this.$store.dispatch('setFooterMenus', result.ReturnValue.FooterMenus);
      })
      .catch(error => {
        console.log(error);
      });
  }

  search () {
    switch (this.searchType) {
      case 0:
        this.searchPro();
        break;
      case 1:
        this.searchCms();
        break;
    }
  }

  searchPro () {
    this.$store.dispatch('setSearchKey', this.key);
    if (this.key !== '') {
      this.$router.push({
        path: '/product/search',
        name: 'productSearch',
        params: {
          key: this.key
        }
      });
    } else {
      this.$router.push({
        path: '/product/search/-'
      });
    }
  }

  searchCms () {
    this.$router.push({
      path: '/cms/search',
      name: 'cmsSearch',
      params: {
        key: this.key
      }
    });
  }

  get currentlang () {
    return this.$i18n.locale;
  }
  private changLange (lang) {
    this.$Api.member
      .setUILanguage(lang)
      .then(result => {
        this.$i18n.locale = lang;
        localStorage.setItem('locale', lang);
        this.Reload();
      })
      .catch(error => {
        console.log(error);
      });
  }

  created () {
    this.$store.dispatch('setShopCart', this.$Api.shoppingCart.getShoppingCart());
  }

  mounted () {
    this.getMenu();
  }
}
</script>

<style scoped lang="less">
.showMenuYes{
  height:151px;
  transition:all 1s;
}
#header{
  z-index: 9999;
  top:0px;
  width: 100%;
  display: flex;
}
.headerBg{
  width: 100%;
  background:#fff;
  background-size: cover;
  display: block;
  position: relative;
  z-index: 1000;
  //  box-shadow: 0 0 10px 0 #d4d5d1;
}
.headerTop{
    width: 100%;
    margin: 0 auto;
    // padding-top: 10px;
    height: 40px;
    background-color: #223440;
}
.headerTop .inner{
    width: 1200px;
    margin: 0 auto;
    // position: relative;
    .left{
      float: left;
      p{
        color: #fff;
        font-size: 14px;
        line-height: 40px;
      }
    }
    .right{
      float: right;
      display: flex;
      justify-content: center;
      align-items: center;
    }
}
.headerBottom{
  width: 100%;
  height: 100px;
  background-color: #2f4858;
  box-shadow: 0 0 5px #323232;
      position: relative;
    z-index: 10;
  .inner{
    width: 1200px;
    height: 100px;
    margin: 0 auto;
    position: relative;
    // display: flex;
    // justify-content: space-between;

    .logoBox{
        // width: 100%;
        text-align: center;
        display: flex;
        align-items: center;
        justify-content: center;
        // margin-top: 18px;
        // margin-left: -6px;
        float: left;
        position: absolute;
        left: -6px;
        bottom: -20px;
    }
    .logoBox a{
        display: flex;
    }
    .logoBox a img{
      width: 100%;
    }
    /deep/ .header_menu{
      width: 854px;
      float: right;
    }
  }
}
.memberLogin{
    display: flex;
    float: left;
    align-items: center;
    position: relative;
    margin-right: 17px;
}
.cartTop{
    display: flex;
    float: left;
    align-items: center;
    position: relative;
    margin-right: 17px;
    a{
      height: 20px;
    }
}
.langBox{
    display: flex;
    float: left;
    align-items: center;
    position: relative;
    justify-content: center;
    margin-left: 10px;
}
.langBox a{
   color:#323232;
   font-size: 14px;
    line-height: 32px;
    margin-right: 10px;
    width: 40px;
    display: inline-block;
    text-align: center;
}
.langBox a:nth-child(2){
    margin-right: 0px!important;
}
.langActive{
    background: #808080;
    color:#FFF!important;
}

.fav-icon {
    background: url('/images/pc/pcindex_08.png') no-repeat center center;
    display: inline-block;
    width: 20px;
    height: 20px;
    background-size: contain;
}
.ptxicon {
    background: url('/images/pc/ptx.png') no-repeat center center;
    display: inline-block;
    width: 20px;
    height: 20px;
    background-size: contain;
}
// new css
.header-layout {
 /deep/ .header_menu {
  //  width: 1200px;
  //  margin: 30px auto 10px;
  float: right;
   > ul {
     > li {
      float: left;
      display: flex;
      align-items: center;
      position: relative;
      width: auto;
      padding: 0 20px;
      &::after{
        content: '';
        width: 1px;
        height: 20px;
        background-color: #6b7a84;
        position: absolute;
        right: 0;
        top: 50%;
        transform: translateY(-50%);
      }
      &:last-child{
        padding-right: 0;
        &::after{
           content: '';
          display: none;
        }
      }
      > a {
        width: 100%;
        font-size: 18px;
        color: #d5dee4;
        display: block;
        text-align: center;
        font-weight: 500;
        text-transform: uppercase;
        padding: 0 12px;
        height: 100px;
        // line-height: 100px;
        display: flex;
        align-items: center;
      }

      &:hover{
        > a  {
          background: linear-gradient(#dc2910, #c42510, #9c200f);
          color: #fff;
        }
        > ul{
          width: 130px;
          left: 0;
        }
      }

      ul {
        // box-shadow: 0 4px 5px #ebebeb;
        box-shadow: none;
        border: 1px solid #ebebeb;
        border-top: none;
        li {
          border: 0;
          position: relative;
          border-bottom: 1px solid #ebebeb;
          &:last-child{
            border-bottom: none;
          }
          > a {
            font-size: 18px;
            color: #666666;
            display: block;
            text-align: center;
            font-weight: 500;
            text-transform: uppercase;
            padding: 10px 5px;
          }

          &:hover{
            &::before{
              content: '';
              width: 6px;
              height: 100%;
              background-color: #de2910;
              top: 0;
              left: -1px;
              position: absolute;
            }
            background: none;
             > a {
              background: #fff;
              color: #de2910;
            }
          }
        }
      }
     }
   }
 }
}

.search-box {
  float: left;
  display: flex;
  align-items: center;
margin-top: 5px;
margin-right: 20px;
  /deep/ .el-select {
    width: 72px;
  margin-right: 1px;

    .el-input__inner {
      height: 30px;
      line-height: 30px;
      border-radius: 0;
          padding-right: 20px;
    padding-left: 10px;
    border-top-left-radius: 3px;
    border-bottom-left-radius: 3px;

    }

    .el-input__icon {
      line-height: 26px;
      width: 18px;
      color: #2f4858;
      // margin-top: -3px;
    }
    .el-input__suffix{
      top: 0;
    }
  }

  .search-input {
    border: none;
    width: 217px;
    display: flex;
    float: left;
    align-items: center;
    background-color: #fff;
    border-top-right-radius: 3px;
    border-bottom-right-radius: 3px;

    > input {
      width: 217px;
      float: left;
      border:none;
      background: transparent;
      line-height: 30px;
      text-indent: 10px;
      outline: 0;
      box-sizing: border-box;
    height: 30px;
    }

    .searchBtn{
      width: 25px;
      height: 25px;
      display: inline-block;
      background: url('/images/pc/pcindex_03.png') no-repeat center center;
      background-size: 100%;
      cursor: pointer;
      margin-right: 10px;
    }
  }
}
.header-layout .ENG{
  .headerBottom {
    /deep/ .header_menu{
      width: 812px;
      >ul{
        display: block;
        >li{
          &:nth-child(4){
            >a{
              width: 120px;
            }
          }
          >a{
            text-transform: capitalize;
            color: #d5dee4;
          }
          &:hover{
            ul{
              width: auto;
              left: 20px;
              white-space: nowrap;
              li{
                a{
                  text-transform: capitalize;
                  padding: 10px 10px;
                }
              }
            }
          }
        }
      }
    }
  }
  .search-box{
    /deep/ .el-select{
      width: 90px;
    }
  }
}
</style>
