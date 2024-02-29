<template>
  <div class="liveBox" style="text-align: center;">
    <div class="liveBox_in">
      <div class="title">
        <h1>{{conetntTitle}}</h1>
      </div>
      <ul>
        <li v-for="(cms,index) in contentList" :key="index">
          <div class="img"></div>
          <div class="title">{{cms.Title}}</div>
          <div class="Desc">{{cms.Desc}}</div>
        </li>
      </ul>
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
      pageSize: 9, // 每页显示条目个数
      totalRecord: 0 // 总条目数
    }
  private SortOrder: string = 'asc';
  private SortName: string = 'CreateDate'

  get isMobile () {
    return this.$store.state.isMobile;
  }
  getadvantage () {
    this.$Api.cms.getFromContentByCatId(40134, this.pager.currentPage, this.pager.pageSize, this.isMobile, this.SortName, this.SortOrder).then(result => {
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
    margin-top: 10px;
    padding-top: 40px;
    padding-bottom: 50px;
    >.title{
      height: 100px;
      background: url(/images/pc/index_21.png) no-repeat center center;
      position: relative;
      &::before{
          content: '';
          width: 484px;
          height: 1px;
          background-color: #e3e6e8;
          position: absolute;
          left: 0;
          top: 50%;
          transform: translateY(-50%);
        }
        &::after{
          content: '';
          width: 484px;
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
        letter-spacing: 2px;

      }
    }
    ul{
      margin: 0 40px;
      display: flex;
      justify-content: space-between;
      margin-top: 35px;
      li{
        // float: left;
        width: 200px;
        height: 290px;
        background-color: #ffffff;
        border-radius: 100px;
        text-align: center;
        transition: all 0.3s;
        .title{
          font-size: 26px;
          color: #2f4858;
          font-weight: bold;
          margin-bottom: 20px;
        }
        .img{
          height: 130px;
        }
        .Desc{
          font-size: 18px;
          color: #333333;
        }
        &:nth-child(1){
          .img{
            width: 70px;
            // height: 70px;
            background: url(/images/pc/advantage01.png) no-repeat center center;
            background-size: contain;
            margin: 0 auto;
          }
        }
        &:nth-child(2){
          .img{
            width: 70px;
            // height: 70px;
            background: url(/images/pc/advantage02.png) no-repeat center center;
            background-size: contain;
            margin: 0 auto;
          }
        }
        &:nth-child(3){
          .img{
            width: 63px;
            // height: 74px;
            background: url(/images/pc/advantage03.png) no-repeat center center;
            background-size: contain;
            margin: 0 auto;
          }
        }
        &:nth-child(4){
          .img{
            width: 70px;
            // height: 70px;
            background: url(/images/pc/advantage04.png) no-repeat center center;
            background-size: contain;
            margin: 0 auto;
          }
        }
        &:nth-child(5){
          .img{
            width: 76px;
            // height: 76px;
            background: url(/images/pc/advantage05.png) no-repeat center center;
            background-size: contain;
            margin: 0 auto;
          }
        }
        &:hover{
          box-shadow: 5px 5px 10px #e4e7e9;
          .title{
            color: #de2910;
          }
          &:nth-child(1){
          .img{
            background: url(/images/pc/advantage01-hover.png) no-repeat center center;
          }
        }
        &:nth-child(2){
          .img{
            background: url(/images/pc/advantage02-hover.png) no-repeat center center;
          }
        }
        &:nth-child(3){
          .img{
            background: url(/images/pc/advantage03-hover.png) no-repeat center center;
          }
        }
        &:nth-child(4){
          .img{
            background: url(/images/pc/advantage04-hover.png) no-repeat center center;
          }
        }
        &:nth-child(5){
          .img{
            background: url(/images/pc/advantage05-hover.png) no-repeat center center;
          }
        }
        }
      }
    }
}
</style>
