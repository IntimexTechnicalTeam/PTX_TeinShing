<template>
  <div class="map-main" >
    <div class="inner">
      <div class="title">
          <h1>{{titles}}</h1>
        </div>
      <div class="maps">
        <div v-html="mapContent.Body"></div>
        <div v-html="Content.Body"></div>
      </div>
    </div>

  </div>
</template>
<script lang="ts" scoped>
import { Component, Prop, Vue } from 'vue-property-decorator';
@Component
export default class PkMap extends Vue {
  Content:string='';
  mapContent:string='';
  titles:string='';
  getFbContent () {
    this.$Api.cms.getContentByDevice({ key: 'contactus', IsMobile: false }).then(result => {
      this.Content = result.CMS;
      this.titles = result.CMS.Title;
    });
    this.$Api.cms.getContentByDevice({ key: 'maps', IsMobile: false }).then(result => {
      this.mapContent = result.CMS;
    });
  }
  mounted() {
   this.getFbContent();
  }
}
</script>

<style scoped lang="less">
.map-main {
  width: 100%;
  margin: 0 auto;
  // display: flex;
  background-color: #f7f8f8;
  padding: 50px 0;
  box-sizing: border-box;
  .inner{
    width: 1200px;
    margin: 0 auto;
  }
  .title{
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
    .maps{
      margin-top: 30px;
      /deep/ iframe{
        border: none;
      }
      /deep/ .contact_box{
        ul{
          display: flex;
          justify-content: space-between;
          text-align: center;
          margin-top: 30px;
          li{
            .text{
              display: flex;
              align-items: center;
              font-size: 20px;
              color: #2f4858;
              font-weight: bold;
              margin-bottom: 15px;
              justify-content: center;
              img{
                margin-right: 10px;
              }
            }
            p{
              font-size: 20px;
              color: #666666;
            }
          }
        }
      }
    }
}
.map-main img {
  width: 100%;
}
</style>
