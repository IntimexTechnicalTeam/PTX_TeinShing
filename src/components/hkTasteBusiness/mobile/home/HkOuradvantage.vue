<template>
  <div class="liveBox" style="text-align: center;">
    <div class="liveBox_in fix">
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
    width:100%;
    margin: 0 auto;
    margin-top: 1rem;
    padding: 2rem 1.5rem;
    box-sizing: border-box;
    >.title{
      height: 7rem;
      background: url(/images/pc/index_21.png) no-repeat center center;
      position: relative;
      background-size: contain;
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
    ul{
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      margin-top: 2rem;
      li{
        float: left;
        width: 14rem;
        height: 14rem;
        background-color: #ffffff;
        border-radius: 14rem;
        text-align: center;
        transition: all 0.3s;
        margin-bottom: 1.5rem;
        box-shadow: 2px 5px 5px #e6e7e9;
        &:nth-child(2n){
          // float: right;
          margin-left: 1rem;
        }
        .title{
          font-size: 1.6rem;
          color: #de2910;
          font-weight: bold;
          margin-bottom: 0.5rem;
          margin-top: 1rem;
        }

        .Desc{
          font-size: 1.2rem;
          color: #333333;
        }
        &:nth-child(1){
          .img{
            width: 4rem;
            // height: 70px;
            background: url(/images/pc/advantage01-hover.png) no-repeat center center;
            background-size: contain;
            margin: 0 auto;
          }
        }
        &:nth-child(2){
          .img{
            width: 4rem;
            // height: 70px;
            background: url(/images/pc/advantage02-hover.png) no-repeat center center;
            background-size: contain;
            margin: 0 auto;
          }
        }
        &:nth-child(3){
          .img{
            width: 4rem;
            // height: 74px;
            background: url(/images/pc/advantage03-hover.png) no-repeat center center;
            background-size: contain;
            margin: 0 auto;
          }
        }
        &:nth-child(4){
          .img{
            width: 4rem;
            // height: 70px;
            background: url(/images/pc/advantage04-hover.png) no-repeat center center;
            background-size: contain;
            margin: 0 auto;
          }
        }
        &:nth-child(5){
          .img{
            width: 4rem;
            // height: 76px;
            background: url(/images/pc/advantage05-hover.png) no-repeat center center;
            background-size: contain;
            margin: 0 auto;
          }
        }
        .img{
          height: 4rem;
          margin-top: 1.5rem !important;
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
