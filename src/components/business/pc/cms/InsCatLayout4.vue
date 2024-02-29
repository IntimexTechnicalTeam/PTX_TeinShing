<template>
    <div class="cms-list">
        <div class="tags-box fix">
            <div class="Path_left">
              <p class="PathData">
                <router-link to="/" class="HomePath">{{$t('Message.HomeTips')}}</router-link>
                <ul>
                  <li v-for="(path,index) in cmsData[0].Category.CatPaths" :key="index">
                    <i class="el-icon-arrow-right"></i><span class="currentTitle">{{path.PathName}}</span>
                  </li>
                </ul>
              </p>
            </div>
            <div class="tags_right">
              <ul>
                <li v-for="(tabs,index1) in tabData.Children" :key="index1">
                  <router-link :to="'/cms/catDetail/'+tabs.Id">
                    {{tabs.Name}}
                  </router-link>
                </li>
              </ul>
            </div>
          </div>
        <ul>
            <li v-for="(cms,index) in cmsData" :key="index">
                <router-link :to="'/cms/content/'+cms.Id">
                    <div class="cover">
                        <img :src="cms.Cover" alt=""/>
                    </div>
                    <div class="introduce">
                        <p class="title">{{cms.Title}}</p>
                        <p class="createDate">{{cms.CreateDate}}</p>
                        <p class="desc" v-html="cms.Body"></p>
                        <!-- <p class="viewMore"><span>{{$t('Message.ViewDetail')}}</span></p> -->
                    </div>
                </router-link>
            </li>
        </ul>

        <div class="pager" v-if="pager.totalRecord > pager.pageSize">
            <ins-page :total="pager.totalRecord" v-model="pager.currentPage" :pageNum="pager.pageSize"></ins-page>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
@Component({
  components: {
    InsPage: () => import('@/components/base/pc/InsPage.vue')
  }
})
export default class InsCatLayout1 extends Vue {
    @Prop({ default: () => [] }) private cmsData!: object[]; // cms内容列表数据
    @Prop({ default: () => {} }) private pager!: object; // 分頁器數據
    @Prop({ default: () => {} }) private tabData!: object; //
}
</script>

<style scoped lang="less">
.cms-list {
    width: 1200px;
    margin: 0 auto;
    margin-bottom: 50px;
    >ul{
        display: flex;
        flex-wrap: wrap;
        margin-top: 30px;

        >li{
        width: 380px;
        text-align: center;
        transition: all 0.3s;
        // overflow: hidden;
        position: relative;
        border: 1px solid #f0f0f0;
        box-sizing: border-box;
        // border-radius: 3px;
        padding: 14px;
        margin-right: 30px;
        &:nth-child(3n){
            margin-right: 0%!important;
        }
        a{
            display:block;
            transition: all .3s ease;
            color: #2f4858;
        }
        &:hover{
            border: 1px solid #de2910;
            .introduce{
                .title{
                    color: #de2910;

                }
            }
            }
        .introduce{
            width: 90%;
            margin: 0 auto;
            padding-bottom: 20px;
            .title{
            font-size:18px;
            color:#2f4858;
            margin-bottom: 10px;
            font-weight: bold;
            text-overflow: -o-ellipsis-lastline;
            overflow: hidden;
            text-overflow: ellipsis;
            display: inline-grid;
            -webkit-line-clamp: 2;
            line-clamp: 2;
            -webkit-box-orient: vertical;
            height: 40px;
            margin-top: 25px;
            line-height: 20px;
            align-items: center;
            }
            .createDate{
            font-size: 16px;
            color:#999999;
            padding-bottom: 10px;
            }
            .desc{
              height: 125px;
              overflow: hidden;
                /deep/ p{
                    font-size: 16px;
          color: #999999;
          line-height: 25px;
          text-align: left;
          margin-bottom: 10px;
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
                padding: 5px 20px;
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
}
.tags-box{
  width: 1200px;
  margin: 0 auto;
  // display: flex;
  // justify-content: space-between;
  margin-bottom: 25px;
  margin-top: 20px;
  .Path_left{
    float: left;
    .PathData {
      // width: 1200px;
      // margin: 0 auto;
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      // margin-bottom: 1rem;
      height: 35px;
      span,a,i{
        font-size: 16px;
      }
      .HomePath {
        color: #999999;
      }
      .currentTitle {
        color:#999999;
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
  .tags_right{
    float: right;
    ul{
      li{
        float: left;
        margin-left: 10px;
        a{
          border: 1px solid #e6e6e6;
          font-size: 16px;
          padding: 0 7px;
          height: 35px;
          line-height: 33px;
          box-sizing: border-box;
          display: block;
          color: #999999;
          border-radius: 2px;
          &.router-link-active{
            border: 1px solid #de2910;
            color: #de2910;
          }
        }
      }
    }
  }
}
</style>
