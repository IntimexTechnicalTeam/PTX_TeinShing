<template>
  <div class="liveBox">
    <div class="liveBox_in">
      <div class="title">
        <h1>{{$t("home.Corporateenvironment")}}</h1>
      </div>
      <div class="swiper">
        <swiper :options="swiperOptionCMS" ref="mySwiper" v-if="contentList.length>0">
          <!-- slides -->
          <swiperSlide v-for="(cms,index) in contentList" :key="index">
            <div class="img"><img :src="cms.Cover" alt=""></div>
              <div class="title">
                <p>{{cms.Title}}</p>
              </div>
          </swiperSlide>
        </swiper>
      </div>

      <div class="more">
        <a href="/cms/catDetail/40124">{{ $t("home.ViewMore") }}</a>
      </div>
    </div>
  </div>
</template>
<script lang="ts" scoped>
import { Component, Prop, Vue } from 'vue-property-decorator';
import { swiper, swiperSlide } from 'vue-awesome-swiper/src';
@Component({
  components: {
    swiper,
    swiperSlide
  }
})
export default class PkLiveBox extends Vue {
  contentList: object[] = []; // cms内容列表
  conetntTitle: string = '';
  catId: number = 0; // Tree点击获取内容列表的目录id
  PageStyle: string = '0'; // catDetail页面类型
    pager: any = {
      currentPage: 1, // 当前页
      pageSize: 4, // 每页显示条目个数
      totalRecord: 0 // 总条目数
    }
  private SortOrder: string = 'asc';
  private SortName: string = 'CreateDate'
  swiperOptionCMS: object = {
    slidesPerView: 2,
      spaceBetween: 10,
      pagination: {
        el: '.swiper-pagination.swiper-paginationCMS',
        clickable: true
      }
    };
  get isMobile () {
    return this.$store.state.isMobile;
  }
  getadvantage () {
    this.$Api.cms.getFromContentByCatId(40124, this.pager.currentPage, this.pager.pageSize, this.isMobile, this.SortName, this.SortOrder).then(result => {
        this.contentList = result.Data;
        this.conetntTitle = result.Data[1].Category.Name;
        console.log(result, 'result.Data');
      });
  }
  mounted() {
    this.getadvantage();
  }
}
</script>

<style scoped lang="less">
/*live*/
.liveBox {
    width: 100%;

    background-color: #f7f8f8;
}
.liveBox_in {
    /*width: 75%;*/
    width:100%;
    margin: 0 auto;
    // margin-top: 10px;
    padding-top: 2rem;
    padding-bottom: 4rem;

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
      }
    }
    .swiper{
      display: flex;
      justify-content: space-between;
      margin-top: 35px;
      .swiper-slide{
        // float: left;
        // height: 285px;
          // width: 285px;
        text-align: center;
        transition: all 0.3s;
        overflow: hidden;
        position: relative;
        border-radius: 3px;
        >.title{

          position: absolute;
          left: 0;
          bottom: 0;
          width: 100%;
          height: 4rem;
          text-align: center;
          background-color: rgba(0, 0, 0, 0.6);
          transition: all 0.3s;
          p{
            font-size: 1.3rem;
            color: #fff;
            line-height: 4rem;
          }
        }
        .img{
          height: 16rem;
          width: 100%;
          img{
            width: 100%;
            height: 16rem;
            display: block;
            object-fit: cover;
            object-position: 50% 50%;
          }
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
}
</style>
