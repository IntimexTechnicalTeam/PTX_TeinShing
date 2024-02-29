<template>
  <div class="aboutBox">
    <div class="about01">
      <div class="homeTitle">
        <h1>
          {{ aboutTitle }}
        </h1>
      </div>
    </div>

    <div class="aboutBody fix">
      <div class="left">
        <div class="aboutBodyIn" v-html="aboutContent.Body"></div>
        <div class="more">
          <a href="/cms/catDetail/40122">{{ $t("home.ViewMore") }}</a>
        </div>
      </div>
      <div class="right">
        <img :src="aboutContent.Cover" alt="" />
      </div>
    </div>
  </div>
</template>
<script lang="ts" scoped>
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
@Component
export default class HomeAbout extends Vue {
  @Prop() private homeData!: object;
  aboutContent: string = '';
  aboutTitle: string = '';
  getAboutContent() {
    this.$Api.cms
      .getContentByDevice({ key: 'aboutus', IsMobile: false })
      .then((result) => {
        this.aboutContent = result.CMS;
        this.aboutTitle = result.CMS.Title;
      });
  }
  mounted() {
    this.getAboutContent();
  }
}
</script>
<style lang="less" scoped>
.aboutBox {
  width: 100%;
  margin: 0 auto;
  background: url(/images/pc/index_46.png) no-repeat left bottom;
  // background-size: contain;
  .about01 {
    width: 1200px;
    margin: 0 auto;
    margin-top: 50px;
    margin-bottom: 24px;
  }
  .aboutBody {
    width: 1200px;
    margin: 0 auto;
    padding-bottom: 44px;
    .left{
      width: 520px;
      float: left;
    }
    .right{
      width: 650px;
      float: right;
      img{
        border: 6px solid #fff;
        border-radius: 6px;
        box-shadow: 0 0 5px #e6e8e9;
        display: block;
      }
    }
    .aboutBodyIn {
      // height:17.25rem;
      display: block;
      display: -webkit-box;
      -webkit-line-clamp: 15;
      -webkit-box-orient: vertical;
      word-wrap: break-word;
      overflow: hidden;
      /deep/ p {
        display: block;
        display: -webkit-box;
        -webkit-line-clamp: 15;
        -webkit-box-orient: vertical;
        word-wrap: break-word;
        overflow: hidden;
        text-align: justify;
        font-size: 18px;
        color: #666666;
      }
    }
    p,
    a,
    div,
    span,
    strong {
      font-size: 18px;
      color: #666666;
      line-height: 24px;
    }
    .more {
      text-align: left;
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
  .homeTitle {
    height: 100px;
    background: url(/images/pc/index_21.png) no-repeat center center;
    position: relative;
    &::before {
      content: "";
      width: 503px;
      height: 1px;
      background-color: #e3e6e8;
      position: absolute;
      left: 0;
      top: 50%;
      transform: translateY(-50%);
    }
    &::after {
      content: "";
      width: 503px;
      height: 1px;
      background-color: #e3e6e8;
      position: absolute;
      right: 0;
      top: 50%;
      transform: translateY(-50%);
    }
    h1 {
      line-height: 100px;
      font-size: 32px;
      font-weight: bold;
      color: #2f4858;
      position: relative;
      text-align:center;
      letter-spacing: 2px;
    }
  }
}
</style>
