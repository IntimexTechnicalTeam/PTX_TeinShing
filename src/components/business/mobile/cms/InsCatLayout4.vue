<template>
    <div class="cms-list">
        <div class="tags-box fix">
            <div class="TabData">
            <ul>
              <li v-for="(tabs,index1) in tabData.Children" :key="index1">
                <router-link :to="'/cms/catDetail/'+tabs.Id">
                  {{tabs.Name}}
                </router-link>
              </li>
            </ul>
          </div>
          <p class="PathData">
            <router-link to="/" class="HomePath">{{$t('Message.HomeTips')}}</router-link>
            <ul>
              <li v-for="(path,index) in cmsData[0].Category.CatPaths" :key="index">
                <i class="el-icon-arrow-right"></i><span class="currentTitle">{{path.PathName}}</span>
              </li>
            </ul>
          </p>
        </div>
        <ul>
            <li v-for="(cms,index) in cmsData" :key="index">
                <router-link :to="'/cms/content/'+cms.Id">
                    <div class="cover">
                        <img :src="cms.Cover" alt=""/>
                    </div>
                    <div class="introduce">
                        <p class="title">{{cms.Title}}</p>
                        <p class="createDate">{{cms.Desc}}</p>
                        <!-- <p class="desc">{{cms.Desc}}</p> -->
                        <p class="desc" v-html="cms.Body"></p>
                        <!-- <p class="viewMore"><span>{{$t('Message.ViewDetail')}}</span></p> -->
                    </div>
                </router-link>
            </li>
        </ul>

        <!-- <div class="loading_box" ref="load" @touchstart="loading" v-if="pager.totalRecord > pager.pageSize">
            <span class="loading_title">{{tips?$t('Action.LoadMore'):$t('home.Thatsall')}}</span>
        </div>
        <div class="loading_box" v-else>
            <span class="loading_title">{{$t('home.Thatsall')}}</span>
        </div> -->
        <div class="pager" v-if="pager.totalRecord > pager.pageSize">
            <ins-page :total="pager.totalRecord" v-model="pager.currentPage" :pageNum="pager.pageSize"></ins-page>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
@Component({
  components: {
    InsPage: () => import('@/components/base/mobile/InsPage.vue')
  }
})
export default class InsCatLayout1 extends Vue {
    @Prop({ default: () => [] }) private cmsData!: object[]; // cms内容列表数据
    @Prop({ default: () => {} }) private pager!: any; // 分頁器數據
     @Prop({ default: () => {} }) private tabData!: object; //

    private tips:boolean = true;
    private LoadingInstance!: any;

    // loading (e) {
    //   if (this.tips) {
    //     this.LoadingInstance = this.$loading({
    //       target: this.$refs.load as any,
    //       fullscreen: false,
    //       // spinner: 'el-icon-loading',
    //       text: 'Loading...'
    //     });
    //     setTimeout(() => {
    //       this.load();
    //       this.LoadingInstance.close();
    //     }, 2000);
    //   }
    // }
    // load () {
    //     if (this.pager.totalRecord !== this.cmsData.length) {
    //       this.pager.currentPage++;
    //     } else {
    //         this.tips = false;
    //     }
    // }
}
</script>

<style scoped lang="less">
.cms-list {
    >ul {
        width: 90%;
        margin: 0 auto;

        >li{
        //  width: 23%;
        width: 100%;
        //  float: left;
        //  margin-right: 2.66%;
        margin-bottom: 2rem;
        //  &:nth-child(4n){
        //    margin-right: 0%!important;
        //  }
        a{
            display:block;
            border:1px solid #eee;
            padding: 0.3rem;
        //  transition: all .3s ease;
        // -o-transition: all .3s ease;
        // -webkit-transition: all .3s ease;
        // -moz-transition: all .3s ease;
        //  &:hover{
        //    border:1px solid #00d0b5;
        //    .introduce{
        //      .title{
        //        color:#00d0b5;
        //      }
        //    }
        //  }
        }
        .introduce{
            width: 96%;
            margin: 0 auto;
            //  padding-bottom: 20px;
            padding-bottom: 1rem;
            .title{
            //  font-size:18px;
            font-size: 1.4rem;
            font-weight: bold;
            // padding-bottom: 10px;
            // border-bottom: 1px solid #eee;
            color:#2f4858;
            margin-bottom: 0.5rem;
            text-overflow: -o-ellipsis-lastline;
            overflow: hidden;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            line-clamp: 2;
            -webkit-box-orient: vertical;
            max-height: 46px;
            margin-top: 1rem;
            text-align: center;
            }
            .createDate{
            //  font-size: 16px;
            font-size: 1.2rem;
            color:#999999;
            padding-bottom: 10px;
            text-align: center;
            }
            // .desc{
            // //  font-size: 14px;
            // font-size: 1.2rem;
            // color:#000000;
            // //  margin-bottom: 40px;
            // margin-bottom: 2.5rem;
            // text-overflow: -o-ellipsis-lastline;
            // overflow: hidden;
            // text-overflow: ellipsis;
            // display: -webkit-box;
            // -webkit-line-clamp: 2;
            // line-clamp: 2;
            // -webkit-box-orient: vertical;
            // // max-height: 36px;
            // }
            .desc{
              height: 8rem;
              overflow: hidden;
                /deep/ p{
                    font-size: 1.2rem;
                    color: #999999;
                    line-height: 1.8rem;
                    text-align: left;
                    // margin-bottom: 10px;
                    text-overflow: -o-ellipsis-lastline;
                    overflow: hidden;
                    text-overflow: ellipsis;
                    display: -webkit-box;
                    -webkit-line-clamp: 4;
                    line-clamp: 4;
                    -webkit-box-orient: vertical;
                }
            }
            .viewMore{
            text-align: center;
            display: inline-block;
            width: 100%;
            span{
                //padding: 5px 20px;
                padding: 0.4rem 2.8rem;
                font-size: 1.32rem;
                font-weight: bold;
                background: @base_color;
                display: inline-block;
                color:#fff;
            }
            }
        }
        .cover{
            width: 100%;
            img{
            width: 100%;
            }
        }
        }
    }

    .loading_box{
        position: relative;
        text-align: center;
        margin-bottom: 7.5rem;
        margin-top: 6rem;

        &:before{
            background:#ddd;
            content:"";
            display: block;
            height: 1px;
            position: absolute;
            top: 50%;
            width: 100%;
        }

        .loading_title{
        background: #fff;
        padding: 0 3rem;
        position: relative;
        z-index: 1;
        font-size: 1.5rem;
        color: #787878;
        }
    }
}
.tags-box{
  width: 100%;
  margin: 0 auto;
  // display: flex;
  // justify-content: space-between;
//   margin-bottom: 2rem;
  margin-top: 1rem;
  .TabData{
  width: 90%;
  margin: 0 auto;

  ul{
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    li{
      a{
        border-radius: 3px;
        border: 1px solid #e6e6e6;
        padding: 0.8rem 1rem;
        box-sizing: border-box;
        margin-right: 0.8rem;
        margin-bottom: 0.8rem;
        color: #666666;
        display: block;
      }
      /deep/ .router-link-active{
        color: #de2910;
        border: 1px solid #de2910;
      }
      &:last-child{
        a{
          margin-right: 0;
        }
      }
    }
  }
}
.PathData {
  width: 90%;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  margin-bottom: 1rem;
  margin-top: 1rem;
  span,a,i{
    font-size: 1.2rem;
  }
  .HomePath {
    color: #666;
  }
  .currentTitle {
    color:#666;
  }
  ul{
    display: flex;
    align-items: center;
    li:last-child{
      span{
        color: #de2910;
      }
    }
  }
}
}
</style>
