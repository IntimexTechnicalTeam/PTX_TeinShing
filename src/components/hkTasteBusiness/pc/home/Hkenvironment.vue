<template>
  <div class="liveBox">
    <div class="liveBox_in">
      <div class="title">
        <h1>{{$t("home.Corporateenvironment")}}</h1>
      </div>
      <ul>
        <li v-for="(cms,index) in contentList" :key="index">
          <div class="img"><img :src="cms.Cover" alt=""></div>
          <div class="title">
            <p>{{cms.Title}}</p>
          </div>
          <div class="text_show">
            <div class="logo"><img src="/images/pc/showlogo.png" alt=""></div>
            <div class="title">{{cms.Title}}</div>
          </div>
        </li>
      </ul>
      <div class="more">
        <a href="/cms/catDetail/40124">{{ $t("home.ViewMore") }}</a>
      </div>
    </div>
  </div>
</template>
<script lang="ts" scoped>
import { Component, Prop, Vue } from 'vue-property-decorator';
@Component
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
    width:1200px;
    margin: 0 auto;
    // margin-top: 10px;
    padding-top: 40px;
    padding-bottom: 60px;
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
        letter-spacing: 2px;
      }
    }
    ul{
      display: flex;
      justify-content: space-between;
      margin-top: 35px;
      li{
        // float: left;
        height: 285px;
          width: 285px;
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
          height: 50px;
          text-align: center;
          background-color: rgba(0, 0, 0, 0.6);
          transition: all 0.3s;
          p{
            font-size: 18px;
            color: #fff;
            line-height: 50px;
          }
        }
        .text_show{
          position: absolute;
          top: -285px;
          left: 0;
          width: 100%;
          height: 285px;
          background-color: rgba(222, 41, 16, 0.9);
          transition: all 0.3s;
          .logo{
            margin-top: 84px;
            text-align: center;
            margin-bottom: 20px;
            img{
              display: block;
              margin-left: auto;
              margin-right: auto;
            }

          }
          .title{
              font-size: 18px;
              color: #fff;
            }
        }
        .img{
          height: 285px;
          width: 285px;
          img{
            width: 100%;
            height: 285px;
            display: block;
            object-fit: cover;
            object-position: 50% 50%;
          }
        }
        .Desc{
          font-size: 18px;
          color: #333333;
        }
        &:hover{

          >.title{
            bottom: -50px;
          }
          .text_show{
            top: 0;
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
}
</style>
