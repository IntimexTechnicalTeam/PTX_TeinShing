<template>
  <div class="home" :class="{'ENG':$Storage.get('locale') === 'E'}" v-cloak>
    <HomeBanner :page="'Home'" />
    <HkOuradvantage/>
    <HkHomeAbout/>
    <Hkenvironment/>
    <PkNews/>
    <HkLiveBox/>
    <PkMap/>
    <HkPromotion/>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
@Component({
  components: {
    HomeBanner: () => import('@/components/base/mobile/InsBanner.vue'),
    HkPromotion: () => import('@/components/hkTasteBusiness/mobile/home/HkPromotion.vue'),
    HkOuradvantage: () => import('@/components/hkTasteBusiness/mobile/home/HkOuradvantage.vue'),
    HkHomeAbout: () => import('@/components/hkTasteBusiness/mobile/home/HkHomeAbout.vue'),
    Hkenvironment: () => import('@/components/hkTasteBusiness/mobile/home/Hkenvironment.vue'),
    PkNews: () => import('@/components/hkTasteBusiness/mobile/home/PkNews.vue'),
    HkLiveBox: () => import('@/components/hkTasteBusiness/mobile/home/HkLiveBox.vue'),
    PkMap: () => import('@/components/hkTasteBusiness/mobile/home/PkMap.vue')
  }
})
export default class InsHome extends Vue {
  swiperOption: object = {
    autoplay: {
      disableOnInteraction: false
    },
    pagination: {
      el: '.swiper-pagination',
      clickable: true
    }
  };

  get AreaCode () {
    return localStorage.getItem('AreaCode') || '';
  }
}
</script>

<style lang="less" scoped>
/deep/ .banner {
  .swiper-pagination-bullet{
    width: 10px!important;
    height: 10px!important;
    background: #e6e6e6;
    opacity: 1;
  }

  .swiper-pagination-bullet-active{
    background: #b19162!important;
  }
}

.activity {
  width: 90%;
  margin: 5rem auto;

  .TitleBg{
    width: 90%;
    height: 5rem;
    border:1px solid #4d4d4d;
    margin: 0 auto;
    padding: .5rem;
    margin-bottom: 4rem;
    .innerBox{
      width: 100%;
      height: 100%;
      background:#4d4d4d;
      color: #FFF;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 2rem;
      font-weight: 700;
      font-family: 'Arial';
    }
  }
}

.ins-calendar {
  margin: 2rem auto;
}
.home.ENG{
  /deep/ .liveBox{
    .liveBox_in{
      .title{
        h1{
          letter-spacing: 0;
          font-size: 1.8rem;
        }
        &::before{
          content: '';
          width: 18%;
        }
        &::after{
          content: '';
          width: 18%;
        }
      }
    }
  }
  /deep/ .aboutBox{
    .homeTitle{
      h1{
        letter-spacing: 0;
          font-size: 1.8rem;
      }
    }
  }
  /deep/ .news {
    .title {
      h1{
        letter-spacing: 0;
          font-size: 1.8rem;
      }

          &::before{
          content: '';
          width: 28%;
        }
        &::after{
          content: '';
          width: 28%;
        }
    }
    .swiper .swiper-slide {
      .news-title{
        overflow: hidden;
        display: -webkit-box;
        -webkit-line-clamp: 2;
        -webkit-box-orient: vertical;
        word-break: break-word;
        -webkit-box-align: center;
        -ms-flex-align: center;
        align-items: center;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        justify-content: center;
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
        height: 3.4rem;
      }
    }
  }
  /deep/ .map-main{
    .inner{
      .title {
      h1{
        letter-spacing: 0;
          font-size: 1.8rem;
      }

          &::before{
          content: '';
          width: 28%;
        }
        &::after{
          content: '';
          width: 28%;
        }
    }
    }
  }
  /deep/ .PromotionMain{
    .productBox{
      .TitleBg{
        .innerBox{
          letter-spacing: 0;
          font-size: 2rem;
        }
        &::before{
          content: '';
          width: 28%;
        }
        &::after{
          content: '';
          width: 28%;
        }
      }
    }
  }
}
</style>
