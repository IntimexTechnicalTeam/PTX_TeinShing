<template>
    <div class="CatMain">
      <!-- <transition name="slide">
        <div key="1" v-if="!waiting" style="display:flex;">
           <div class="DetailTitle"><img :src="BannerImg" v-show="BannerImg!==null"><div class="TitleBg"><div class="innerBoxText">{{CateName}}</div></div></div>
      </div>
      </transition>
      <transition name="slide">
        <div class="faker" key="2" v-if="waiting" v-loading="true"></div>
      </transition> -->
        <div class="NomralBg">
          <div class="TabData">
            <ul>
              <li v-for="(tabs,index1) in TabData.Children" :key="index1">
                <router-link :to="'/cms/catDetail/'+tabs.Id">
                  {{tabs.Name}}
                </router-link>
              </li>
            </ul>
          </div>
          <p class="PathData">
            <!-- <router-link to="/" class="HomePath">{{$t('Message.HomeTips')}}</router-link><i class="el-icon-arrow-right"></i><span class="currentTitle">{{CateName}}</span> -->
            <router-link to="/" class="HomePath">{{$t('Message.HomeTips')}}</router-link>
            <ul>
              <li v-for="(path,index) in CateName.CatPaths" :key="index">
                <i class="el-icon-arrow-right"></i><span class="currentTitle">{{path.PathName}}</span>
              </li>
            </ul>
          </p>
          <div class="cms-list">
              <div class="perData" v-for="(v,index) in ListData" :key="index">
                  <div class="left">
                      <p class="imgs" @click="GoLink(v)"><img :src="v.Cover.indexOf('.png')!==-1 || v.Cover.indexOf('.jpg')!==-1 || v.Cover.indexOf('.jpeg')!==-1 || v.Cover.indexOf('.gif')!==-1?v.Cover:NoImg"></p>
                      <div class="title">
                        <p>
                          {{v.Title}}
                        </p>
                      </div>
                      <!-- <div class="text_show">
                        <div class="logo"><img src="/images/pc/showlogo.png" alt=""></div>
                        <div class="title">{{v.Title}}</div>
                      </div> -->
                  </div>
                  <!-- <div class="right">
                      <p class="title">{{v.Title}}</p>
                      <p class="contentTime">{{v.ContentDateTime}}</p>
                      <p class="desc">{{v.Desc}}</p>
                      <div class="HomeViewMore">
                          <router-link :to="'/cms/contentN/'+v.Id">{{$t('Message.LearnMore')}} >></router-link>
                      </div>
                  </div> -->
              </div>

          </div>
          <div class="pager" v-if="totalRecord > pageSize">
                  <ins-page :total="totalRecord" v-model="currentPage" :pageNum="pageSize"></ins-page>
              </div>
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
    currentPage:number=1;
    pageSize:number=4;
    totalRecord:number=0;
    ListData:any[]=[];
    BannerImg:string='';
    CateName:string='';
    TabData:any[]=[];
    private waiting: boolean = true;
    NoImg:string='/images/pc/proddef.jpg';
      get cid () {
      return this.$route.params.id;
    }
    GoLink(v) {
        // window.location.href = '/cms/contentN/' + v.Id;
    }
    getContentsByCatId () {
        this.$Api.cms.getContentsByCatId(this.cid, this.currentPage, this.pageSize).then((result) => {
            if (result) {
                this.ListData = result.Data;
            }
            result.Data.forEach(function (i) {
            var newDate = new Date(i.ContentDateTime.replace(/-/g, '/'));
            i.ContentDateTime = newDate.getFullYear() + '-' + (newDate.getMonth() + 1) + '-' + newDate.getDate();
            });
            this.totalRecord = result.TotalRecord;
        });
    }
   // 根据设备类型获取CMSCategory信息
  getCategoryByDevice () {
    this.$Api.cms.getCategoryByDevice({ CatId: this.cid, IsMobile: true }).then(async (result) => {
     console.log(result, 'gggggggg');
     this.BannerImg = result.ImagePath;
     this.CateName = result;
     this.getArgument(result.CatPaths[0].CatId);
      this.waiting = false;
    }).catch((error) => {
      console.log(error, 'error');
      this.$message({
        message: error,
        type: 'error'
      });
    });
  }
  getArgument(v) {
    this.$Api.cms.getCategoryByDevice({ CatId: v, IsMobile: true }).then(async (result) => {
      this.TabData = result;
    }).catch((error) => {
      console.log(error, 'error');
      this.$message({
        message: error,
        type: 'error'
      });
    });
  }
    created() {
        this.getContentsByCatId();
        this.getCategoryByDevice();
    }
    @Watch('currentPage', { deep: true })
    onPagerChange() {
      this.getContentsByCatId();
      this.getCategoryByDevice();
    }
}
</script>

<style scoped lang="less">
.faker {
    width: 100%;
    height: 30rem;
}
.CatMain {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    padding-top: 1rem;
  margin-bottom: 3rem;
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
  .DetailTitle{
    width: 100%;
    display: flex;
    flex-wrap:wrap;
    position: relative;
    align-items: center;
    justify-content: center;
    img{
      width: 100%;
    }
    .TitleBg{
      width: 75%;
      border: 1px solid #ffffff;
      height: 4.5rem;
      line-height: 4.5rem;
      margin: 0 auto;
      padding: 10px;
      margin-bottom: 20px;
      top: 50%;
      position: absolute;
      transform: translateY(-50%);
      .innerBoxText{
        background:#ffffff;
        color: #333333;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 2.5rem;
        font-weight: 700;
        font-family: 'Arial';
      }
    }
  }
.cms-list {
    width: 90%;
    margin: 0 auto;
    display: flex;
    flex-wrap: wrap;
    // justify-content: center;
        .perData {
            width: 48%;
            margin-right: 4%;
            float: left;
            display: flex;
            flex-wrap: wrap;
            margin-bottom: 1rem;
            cursor: pointer;
            // &:hover {
            //   .left {
            //     .imgs {
            //       // border:1px solid @base_color;
            //     }
            //     .title{
            //       bottom: -4rem;
            //     }
            //     .text_show{
            //       top: 0;
            //     }
            //   }
            // }
            &:nth-child(2n) {
              margin-right: 0px!important;
            }
            .left {
                width: 100%;
                height: 15rem;
                position: relative;
                overflow: hidden;
                border-radius: 3px;
                .imgs {
                    width: 100%;
                    display: inline-block;
                    border-radius: 3px;
                    overflow: hidden;
                    // border: 1px solid #eee;
                    transition: all .3s;
                    img {
                      width: 100%;
                      height: 15rem;
                      display: block;
                      object-fit: cover;
                      object-position: 50% 50%;
                    }
                }
                >.title {
                    // font-size: 22px;
                    // color: #333333;
                    text-overflow: -o-ellipsis-lastline;
                    overflow: hidden;
                    text-overflow: ellipsis;
                    display: -webkit-box;
                    -webkit-line-clamp: 2;
                    line-clamp: 2;
                    -webkit-box-orient: vertical;
                    // margin-top: .5rem;
                    // margin-bottom: .5rem;
                    position: absolute;
                    left: 0;
                    bottom: 0;
                    width: 100%;
                    height: 3rem;
                    text-align: center;
                    background-color: rgba(0, 0, 0, 0.6);
                    -webkit-transition: all 0.3s;
                    transition: all 0.3s;
                    p{
                      font-size: 1.3rem;
                      color: #fff;
                      // line-height: 3rem;
                      display: flex;
                      justify-content: center;
                      align-items: center;
                      height: 3rem;
                    }
                }
          //       .text_show{
          //         position: absolute;
          //         top: -300px;
          //         left: 0;
          //         width: 100%;
          //         height: 300px;
          //         background-color: rgba(222, 41, 16, 0.9);
          //         -webkit-transition: all 0.3s;
          //         transition: all 0.3s;
          //         .logo{
          //   margin-top: 84px;
          //   text-align: center;
          //   margin-bottom: 20px;
          //   img{
          //     display: block;
          //     margin-left: auto;
          //     margin-right: auto;
          //   }

          // }
          // .title{
          //     font-size: 18px;
          //     color: #fff;
          //     text-align: center;
          //   }
          //       }
            }
            .right {
                width: 55%;
                .title {
                    font-size: 1.2rem;
                    color: #9f1e3c;
                    text-overflow: -o-ellipsis-lastline;
                    overflow: hidden;
                    text-overflow: ellipsis;
                    display: -webkit-box;
                    -webkit-line-clamp: 2;
                    line-clamp: 2;
                    -webkit-box-orient: vertical;
                    margin-top: .5rem;
                    margin-bottom: .5rem;
                }
                .contentTime {
                    font-size: 1.2rem;
                    color: #999999;

                }
                 .desc {
                    font-size: 1.2rem;
                    color: #333333;
                    text-overflow: -o-ellipsis-lastline;
                    overflow: hidden;
                    text-overflow: ellipsis;
                    display: -webkit-box;
                    -webkit-line-clamp: 4;
                    line-clamp: 4;
                    -webkit-box-orient: vertical;
                    margin-top: .5rem;
                    margin-bottom: .5rem;
                }
                .HomeViewMore {
                    width: 100%;
                    display: flex;
                    flex-wrap: wrap;
                    justify-content: flex-end;
                    margin-top: 2rem;
                    a {
                        font-size: 1.2rem;
                        color: #b19162;
                        border-bottom: 1px solid #b19162;
                    }
                }
            }
        }
}
.pager{
  text-align: center;
}
</style>
