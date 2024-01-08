<template>
  <div class="news">
    <div class="title">
      <h1>{{$t("home.News")}}</h1>
    </div>
    <ul>
      <li v-for="(n,index) in lastestContents" :key="index">
        <router-link class="img" :to="'/cms/content/'+n.Id">
          <img :src="n.Cover" class="NewsPart" />
        </router-link>
        <p class="news-title">{{n.Title}}</p>
        <p class="news-date">{{n.CreateDate}}</p>
        <p class="news-text" v-html="n.Body"></p>
      </li>
    </ul>
    <div class="more">
      <a href="/cms/catDetail/40113">{{$t('home.ViewMore')}}></a>
    </div>
  </div>
</template>
<script lang="ts" scoped>
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
@Component
export default class PkNews extends Vue {
  lastestContents: any[] = [];
  private SortOrder: string = 'asc';
  private SortName: string = 'CreateDate'
  pager: any = {
      currentPage: 1, // 当前页
      pageSize: 3, // 每页显示条目个数
      totalRecord: 0 // 总条目数
    }
  get isMobile () {
    return this.$store.state.isMobile;
  }
  getNews () {
    var cond = {
      Page: 1,
      PageSize: 3,
      catId: 40121
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
  width: 1200px;
  margin: 0 auto;
  /*height:800px;*/
  padding-bottom: 50px;
  padding-top: 50px;
  box-sizing: border-box;
  >.title{
      height: 100px;
      background: url(/images/pc/index_21.png) no-repeat center center;
      position: relative;
      &::before{
          content: '';
          width: 503px;
          height: 1px;
          background-color: #e3e6e8;
          position: absolute;
          left: 0;
          top: 50%;
          transform: translateY(-50%);
        }
        &::after{
          content: '';
          width: 503px;
          height: 1px;
          background-color: #e3e6e8;
          position: absolute;
          right: 0;
          top: 50%;
          transform: translateY(-50%);
        }
      h1{
        line-height: 100px;
        font-size: 32px;
        font-weight: bold;
        color: #2f4858;
        position: relative;
        text-align: center;
      }
    }
}
.news ul{
      display: flex;
      justify-content: space-between;
      margin-top: 35px;
      li{
        // float: left;
        // height: 380px;
          width: 380px;
        text-align: center;
        transition: all 0.3s;
        // overflow: hidden;
        position: relative;
        border: 1px solid #f0f0f0;
        box-sizing: border-box;
        // border-radius: 3px;
        padding: 14px;

        .news-title{
          font-size: 18px;
            color: #2f4858;
            line-height: 22px;
            margin-bottom: 10px;
            height: 40px;
        }

        .img{
          height: 210px;
          width: 350px;
          margin-bottom: 26px;
          display: block;
          img{
            width: 100%;
            height: 210px;
            display: block;
            object-fit: cover;
            object-position: 50% 50%;
          }
        }
        .news-text{
          font-size: 16px;
          color: #999999;
          line-height: 25px;
          text-align: left;
          margin-bottom: 10px;
        }
        .news-date{
          font-size: 16px;
          color: #666666;
          margin-bottom: 12px;
        }
        &:hover{
        border: 1px solid #de2910;
        border-radius: 3px;
        .news-title{
          color: #de2910;
          font-weight: bold;
        }
      }
      }

    }
    .more {
      text-align: center;
      margin-top: 30px;
      a {
        color: #de2910;
        border: 1px solid #de2910;
        border-radius: 5px;
        padding: 14px 34px;
        font-size: 16px;
        box-sizing: border-box;
        display: inline-block;
      }
    }

</style>
