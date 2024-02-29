<template>
    <div class="header-box" :class="{'ENG':$Storage.get('locale') === 'E'}">
      <div class="headerTop">
        <p>
          {{$t("home.welcome")}}
        </p>

          <!-- <ins-login />
          <router-link to="/account/GetEnquiry">
                <i class="handle-icon ptxicon"></i>
          </router-link>
          <ins-fav />
          <shopcart class="shoppingcart"/>
          <CodeSelect class="header-code" />
          <ins-lang-switch class="headerLang" /> -->
      </div>
        <div class="flex-box">
          <ins-logo />
          <!-- <ins-menu /> -->
          <!-- <ins-menu :layout="1" /> -->
          <div class="Serarch" v-click-outside="closeDialog">
            <div @click="isShowSearch = !isShowSearch">
              <img src="/images/mobile/Serarch.png" alt="">
            </div>
            <transition name="slide-fade">
              <div class="searchback" v-if="isShowSearch">
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
                      <span class="searchBtn" @click="search"><img src="/images/mobile/searchbtn.png"></span>
                  </div>
                </div>
              </div>
            </transition>
          </div>
          <div class="menushow">
            <img class="slide-menu" src="/images/mobile/Mobile-index_05.png" @click="showSlideMenu" v-show="!this.$store.state.isShowMenu" />
            <img class="close-meun" src="/images/mobile/out.png" @click="showSlideMenu" v-show="this.$store.state.isShowMenu"  />
          </div>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';

@Component({
  components: {
    InsLogo: () => import('@/components/base/mobile/InsLogo.vue'),
    InsLangSwitch: () => import('@/components/business/mobile/header/InsLangSwitch.vue'),
    InsLogin: () => import('@/components/business/mobile/header/InsLogin.vue'),
    InsFav: () => import('@/components/business/mobile/header/InsFav.vue'),
    shopcart: () => import('@/components/business/mobile/header/InsShoppingCart.vue'),
    InsMenu: () => import('@/components/business/mobile/header/InsMenu.vue'),
    CodeSelect: () =>
      import('@/components/business/mobile/header/InsCodeSelect.vue')
  }
})
export default class DefaultHeader extends Vue {
  @Prop() private showInFixed!: boolean;
  private key: string = '';
  private typeList: any[] = [{
    value: 0,
    label: this.$i18n.t('product.Productname')
  }, {
    value: 1,
    label: this.$i18n.t('product.ProductCMS')
  }];

  private searchType: number = 0;
  isShowSearch: boolean = false;

  closeDialog () {
    this.isShowSearch = false;
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
    this.isShowSearch = false;
  }

  searchPro () {
    this.$store.dispatch('setSearchKey', this.key);
    this.$store.dispatch('isShowMenu', false);
    if (this.key !== '') {
      this.$router.push({
        path: '/product/search',
        name: 'productSearch',
        params: {
          key: this.key
        }
      });
    } else {
        this.$store.dispatch('isShowMenu', false);
      this.$router.push({
        path: '/product/search/-'
      });
    }
  }

  searchCms () {
    this.$store.dispatch('isShowMenu', false);
    this.$router.push({
      path: '/cms/search',
      name: 'cmsSearch',
      params: {
        key: this.key
      }
    });
  }

  showSlideMenu () {
    let isShow = !JSON.parse(JSON.stringify(this.menuShow));
    this.$store.dispatch('isShowMenu', isShow);
  }
  get isPtx () {
      if (localStorage.getItem('isPtx') === '0') {
        return false;
      } else {
        return true;
      }
  }
  get menuShow () {
    return this.$store.state.isShowMenu;
  }

  get isMobile () {
    return this.$store.state.isMobile;
  }
  get ShopCart () {
    return this.$store.state.shopCart;
  }
  created() {
    this.$store.dispatch('setShopCart', this.$Api.shoppingCart.getShoppingCart());
  }
}
</script>

<style scoped lang="less">
    .header-box {
      background-color: #2f4858;
      // border-bottom: 1px solid #eee;
      position: relative;
      z-index: 10000;
      .flex-box {
        height: 5rem;
        background-color: #2f4858;
        position: relative;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0 1rem;
        padding-left: 0;

        .logo {
          width: 20rem;
          margin-bottom: -2.2rem;
          position: relative;
          z-index: 1;
          // a{
          //   width: 20rem;
          //   margin-bottom: -2.2rem;
          //   position: relative;
          //   z-index: 1;
          // }
        }

        .slide-menu {
          cursor: pointer;
          width: 2.5rem;
        }
        .close-meun {
          cursor: pointer;
          width: 2rem;
        }
        .searchback{
          height: 7rem;
          width: 100%;
          background-color: #fff;
          position: absolute;
          left: 0;
          top: 5rem;
        }
        .search-box {
    width: 96%;
    height: 3rem;
    margin: 0 auto;
    margin-top: 2rem;
    position: relative;
    overflow: hidden;
    margin-bottom: 2rem;
    border: 1px solid #e6e6e6;
    border-radius: 3px;

  /deep/ .el-select {
    width: 6rem;
    position: absolute;
    left: 0;
    top: 0;

    .el-input__inner {
        height: 2.5rem;
        border: 0;
        border-right: 1px solid #e5e5e5;
        border-radius: 0;
        padding: 0 2rem 0 1rem;
        margin-top: 0.25rem;
    }

    .el-input__icon {
          line-height: 2.5rem;
    font-size: 1.2rem;
    color: #2f4858;
    font-weight: bold;
    }
  }

  .search-input {
    width: 100%;
    height: 100%;

    > input {
        width: 100%;
        height: 100%;
        border: 0;
        padding: 0 20% 0 35%;
        box-sizing: border-box;
        font-size: 1.2rem;
        outline: none;
    }

    .searchBtn{
        width: 3rem;
        height: 3rem;
        // background: #666666;
        display: inline-block;
        cursor: pointer;
        position: absolute;
        right: 0;
        top: 0;
        display: flex;
        align-items: center;
        justify-content: center;
        img{
            width: 50%;
            margin: 0 auto;
            display: block;
        }
    }
  }
}
.menushow{
  width: 2.5rem;
}
.Serarch{
  margin-left: 3rem;
  img{
        width: 2rem;
    display: block;
  }
}
      }
    }
.headerTop{
  width: 100%;
  display: flex;
  background: #223440;
  position: relative;
  // border-bottom: 1px solid #e6e6e6;
  flex-wrap: wrap;
  align-items: center;
  padding-left: 1rem;
  padding-right: 1rem;
  box-sizing: border-box;
  .shoppingcart{
      float: left;
      height: 3.5rem;
      display: flex;
      align-items: center;
      justify-content: center;
    }
  .headerLang{
    width:20%;
    position: absolute;
    right: 0px;
    height: 3.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .header-code{
    width:20%;
    position: absolute;
    right: 20%;
    height: 3.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .ptxicon {
    width: 2.5rem;
    height: 2.5rem;
    background: url('/images/pc/ptx.png') no-repeat center center;
    background-size: contain;
    display: inline-block;
    margin-left: .5rem;
  }
  p{
        font-size: 1.1rem;
    color: #fff;
    padding: 1rem 0;
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
.header-box.ENG{
  .flex-box .search-box{
    /deep/ .el-select{
      width: 8rem;
    }

  }
}
</style>
