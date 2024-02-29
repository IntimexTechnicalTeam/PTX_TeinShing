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
    this.$Api.cms.getContentByDevice({ key: 'contactus', IsMobile: true }).then(result => {
      this.Content = result.CMS;
      this.titles = result.CMS.Title;
    });
    this.$Api.cms.getContentByDevice({ key: 'maps', IsMobile: true }).then(result => {
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
  padding: 2rem 1.5rem;
  box-sizing: border-box;
  .inner{
    width: 100%;
    margin: 0 auto;
  }
  .title{
      height: 7rem;
      background: url(/images/pc/index_21.png) no-repeat center center;
      position: relative;
      background-size: contain;
      text-align: center;
      padding: 0 1.5rem;
      box-sizing: border-box;
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
        text-align: center;
      }
    }
    .maps{
      margin-top: 1rem;
      /deep/ iframe{
        border: none;
      }
      /deep/ .contact_box{
        ul{

          margin-top: 1rem;
          li{
            margin-bottom: 1.5rem;
            .text{
              display: flex;
              align-items: center;
              font-size: 1.6rem;
              color: #2f4858;
              font-weight: bold;
              margin-bottom: 0.5rem;
              justify-content: center;
              img{
                margin-right: 10px;
              }
            }
            p{
              font-size: 1.4rem;
              color: #666666;
              text-align: center;
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
