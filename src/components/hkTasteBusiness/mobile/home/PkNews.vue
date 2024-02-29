<template>
  <div class="news">
    <div class="title">
      <h1>{{$t("home.News")}}</h1>
    </div>
    <div class="swiper">
        <swiper :options="swiperOptionNEWS" ref="mySwiper" v-if="lastestContents.length>0">
          <!-- slides -->
          <swiperSlide v-for="(n,index) in lastestContents" :key="index">
            <router-link class="img" :to="'/cms/content/'+n.Id">
              <img :src="n.Cover" class="NewsPart" />
            </router-link>
            <p class="news-title">{{n.Title}}</p>
            <p class="news-date">{{n.CreateDate}}</p>
            <p class="news-text" v-html="n.Body"></p>
          </swiperSlide>
        </swiper>
      </div>
    <div class="more">
      <a href="/cms/catDetail/40132">{{$t('home.ViewMore')}}</a>
    </div>
  </div>
</template>
<script lang="ts" scoped>
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
import { swiper, swiperSlide } from 'vue-awesome-swiper/src';
@Component({
  components: {
    swiper,
    swiperSlide
  }
})
export default class PkNews extends Vue {
  lastestContents: any[] = [];
  private SortOrder: string = 'asc';
  private SortName: string = 'CreateDate'
  pager: any = {
      currentPage: 1, // 当前页
      pageSize: 3, // 每页显示条目个数
      totalRecord: 0 // 总条目数
    }
  swiperOptionNEWS: object = {
    slidesPerView: 1,
      spaceBetween: 20,
      pagination: {
        el: '.swiper-pagination',
        clickable: true
      }
    };
  get isMobile () {
    return this.$store.state.isMobile;
  }
  getNews () {
    var cond = {
      Page: 1,
      PageSize: 3,
      catId: 40132
    };
    // this.$Api.cms.getFromContentByCatId(40121, this.pager.currentPage, this.pager.pageSize, this.isMobile, this.SortName, this.SortOrder).then(result => {
    //   result.Data.forEach(function (item) {
    //     item.CreateDate = item.CreateDate.substring(
    //       0,
    //       item.CreateDate.indexOf(' ')
    //     );
    //   });
    //   this.lastestContents = result.Data;
    // });
    this.$Api.cms.getLastestContents(cond).then(result => {
      result.Data.forEach(function (item) {
        item.CreateDate = item.CreateDate.substring(
          0,
          item.CreateDate.indexOf(' ')
        );
      });
      this.lastestContents = result.Data;
    });
  }
  mounted () {
    this.getNews();
  }
}
</script>

<style scoped lang="less">
.news {
  width: 100%;
  padding: 0 1.5rem;
  box-sizing: border-box;
  margin: 0 auto;
  /*height:800px;*/
  padding-bottom: 4rem;
  padding-top: 2rem;
  box-sizing: border-box;
  >.title{
      height: 7rem;
      background: url(/images/pc/index_21.png) no-repeat center center;
      position: relative;
      background-size: contain;
      text-align: center;
      padding: 0 1.5rem;
      box-sizing: border-box;
      &::before{
          content: '';
          width: 35%;
          height: 1px;
          background-color: #e3e6e8;
          position: absolute;
          left: 0;
          top: 50%;
          transform: translateY(-50%);
        }
        &::after{
          content: '';
          width: 35%;
          height: 1px;
          background-color: #e3e6e8;
          position: absolute;
          right: 0;
          top: 50%;
          transform: translateY(-50%);
        }
      h1{
        line-height: 7rem;
        font-size: 2rem;
        font-weight: bold;
        color: #2f4858;
        position: relative;
        letter-spacing: 2px;
        text-align: center;
      }
    }
}
.news .swiper{
      display: flex;
      justify-content: space-between;
      margin-top: 2rem;
      .swiper-slide{
        // float: left;
        // height: 380px;
          // width: 380px;
        text-align: center;
        transition: all 0.3s;
        // overflow: hidden;
        position: relative;
        border: 1px solid #f0f0f0;
        box-sizing: border-box;
        // border-radius: 3px;
        padding: 1rem;

        .news-title{
          font-size: 1.4rem;
            color: #2f4858;
            line-height: 1.8rem;
            margin-bottom: 1rem;
            height: 3rem;
            font-weight: bold;
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
        }

        .img{
          height: 20rem;
          width: 100%;
          margin-bottom: 26px;
          display: block;
          img{
            width: 100%;
            height: 20rem;
            display: block;
            object-fit: cover;
            object-position: 50% 50%;
          }
        }
        .news-text{

          margin-bottom: 10px;
          height: 7rem;
          overflow: hidden;
          /deep/ p{
            font-size: 1.2rem;
          color: #999999;
          line-height: 1.8rem;
          text-align: left;
          overflow: hidden;
            display: -webkit-box;
            -webkit-line-clamp: 4;
            -webkit-box-orient: vertical;
            word-break: break-word;
            align-items: center;
            justify-content: center;
            box-sizing: border-box;
          }
        }
        .news-date{
          font-size: 1.2rem;
          color: #666666;
          margin-bottom: 12px;
        }
      }

    }
    .more {
      text-align: center;
      margin-top: 3rem;
      a {
        color: #de2910;
        border: 1px solid #de2910;
        border-radius: 5px;
        padding: 0.8rem 2rem;
        font-size: 1.4rem;
        box-sizing: border-box;
        display: inline-block;
      }
    }

</style>
