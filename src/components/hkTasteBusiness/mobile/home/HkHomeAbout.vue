<template>
  <div class="aboutBox">
    <div class="about01">
      <div class="homeTitle">
        <h1>
          {{ aboutTitle }}
        </h1>
      </div>
    </div>
    <div class="Cover">
        <img :src="aboutContent.Cover" alt="" />
      </div>
    <div class="aboutBody fix">
      <div class="aboutBodyIn" v-html="aboutContent.Body"></div>
        <div class="more">
          <a href="/cms/catDetail/40122">{{ $t("home.ViewMore") }}</a>
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
      .getContentByDevice({ key: 'aboutus', IsMobile: true })
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
  padding: 0 1.5rem;
  box-sizing: border-box;
  // background: url(/images/pc/index_46.png) no-repeat left bottom;
  // background-size: contain;
  .about01 {
    width: 100%;
    margin: 0 auto;
    margin-top: 2rem;
    margin-bottom: 1.5rem;
  }
  .Cover{
    margin-bottom: 2rem;
    border:6px solid #fff;
    box-shadow: 0 0 6px #e6e8e9;
      img{
        width: 100%;
        display: block;
      }
    }
  .aboutBody {
    width: 100%;
    margin: 0 auto;
    padding-bottom: 2rem;

    .aboutBodyIn {
      // height:17.25rem;
      display: block;
      // display: -webkit-box;
      // -webkit-line-clamp: 15;
      // -webkit-box-orient: vertical;
      word-wrap: break-word;
      overflow: hidden;
      /deep/ p {
        display: block;
        // display: -webkit-box;
        // -webkit-line-clamp: 15;
        // -webkit-box-orient: vertical;
        word-wrap: break-word;
        // overflow: hidden;
        text-align: justify;
        font-size: 1.2rem;
        color: #666666;
        line-height: 1.8rem;
        span{
          font-size: 1.2rem;
          line-height: 1.8rem;
        }
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
  .homeTitle {
    height: 7rem;
      background: url(/images/pc/index_21.png) no-repeat center center;
      position: relative;
      background-size: contain;
      text-align: center;
      &::before{
          content: '';
          width: 30%;
          height: 1px;
          background-color: #e3e6e8;
          position: absolute;
          left: 0;
          top: 50%;
          transform: translateY(-50%);
        }
        &::after{
          content: '';
          width: 30%;
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
}
</style>
