<template>
    <div class="CatMain NormalTop">
      <!-- <transition name="slide">
        <div key="1" v-if="!waiting" style="display:flex;">
          <div class="DetailTitle">
            <img :src="BannerImg" v-show="BannerImg!==null">
            <div class="TitleBg"><div class="innerBoxText">{{CateName}}</div></div>
          </div>
      </div>
      </transition>
      <transition name="slide">
        <div class="faker" key="2" v-if="waiting" v-loading="true"></div>
      </transition> -->
        <div class="NomralBg">
          <div class="tags-box fix">
            <div class="Path_left">
              <p class="PathData">
                <router-link to="/" class="HomePath">{{$t('Message.HomeTips')}}</router-link>
                <ul>
                  <li v-for="(path,index) in CateName.CatPaths" :key="index">
                    <i class="el-icon-arrow-right"></i><span class="currentTitle">{{path.PathName}}</span>
                  </li>
                </ul>
              </p>
            </div>
            <div class="tags_right">
              <ul>
                <li v-for="(tabs,index1) in TabData.Children" :key="index1">
                  <router-link :to="'/cms/catDetail/'+tabs.Id">
                    {{tabs.Name}}
                  </router-link>
                </li>
              </ul>
            </div>
          </div>

            <div class="cms-list">
                <div class="perData" v-for="(v,index) in ListData" :key="index">
                    <div class="left">
                        <p class="imgs" @click="GoLink(v)"><img :src="v.Cover.indexOf('.png')!==-1 || v.Cover.indexOf('.jpg')!==-1 || v.Cover.indexOf('.jpeg')!==-1 || v.Cover.indexOf('.gif')!==-1?v.Cover:NoImg"></p>

                        <div class="title">
                          <p>
                            {{v.Title}}
                          </p>
                        </div>
                        <div class="text_show">
                          <div class="logo"><img src="/images/pc/showlogo.png" alt=""></div>
                          <div class="title">{{v.Title}}</div>
                        </div>
                    </div>
                    <!-- <div class="right"> -->

                        <!-- <p class="contentTime">{{v.ContentDateTime}}</p> -->
                        <!-- <p class="desc">{{v.Desc}}</p> -->
                        <!-- <div class="HomeViewMore">
                            <router-link :to="'/cms/contentN/'+v.Id">{{$t('Message.LearnMore')}} >></router-link>
                        </div> -->

                    <!-- </div> -->
                </div>
                <div class="pager" v-if="totalRecord > pageSize">
                    <ins-page :total="totalRecord" v-model="currentPage" :pageNum="pageSize"></ins-page>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import Result from '@/views/pc/InsRegAndPayResult.vue';
import { error } from 'node_modules/_@types_jquery@3.5.29@@types/jquery';
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
@Component({
  components: {
    InsPage: () => import('@/components/base/pc/InsPage.vue')
  }
})
export default class InsCatLayout1 extends Vue {
    currentPage:number=1;
    pageSize:number=6;
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
    this.$Api.cms.getCategoryByDevice({ CatId: this.cid, IsMobile: false }).then(async (result) => {
     console.log(result, 'gggggggg');
     this.BannerImg = result.ImagePath;
     this.CateName = result;
      this.waiting = false;
      this.getArgument(result.CatPaths[0].CatId);
      console.log(result.CatPaths[0].CatId, 'result.CatPaths[0].CatId');
    }).catch((error) => {
      console.log(error, 'error');
      this.$message({
        message: error,
        type: 'error'
      });
    });
  }
  getArgument(v) {
    this.$Api.cms.getCategoryByDevice({ CatId: v, IsMobile: false }).then(async (result) => {
      console.log(result, '是不是拿到父级ID');
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
.pager {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
}
.faker {
    width: 100%;
    height: 30rem;
}
.CatMain {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
}
.NomralBg {
  margin-top: 1rem;
}
.tags-box{
  width: 1200px;
  margin: 0 auto;
  // display: flex;
  // justify-content: space-between;
  margin-bottom: 25px;
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
    width: 500px;
    border: 1px solid #ffffff;
    height: 70px;
    line-height: 70px;
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
      font-size: 40px;
      font-weight: 700;
      font-family: 'Arial';
    }
  }
}
.cms-list {
    width: 1200px;
    margin: 0 auto;
    display: flex;
    flex-wrap: wrap;
        .perData {
            width: 380px;
            margin-right: 30px;
            float: left;
            display: flex;
            flex-wrap: wrap;
            margin-bottom: 30px;
            cursor: pointer;
            &:hover {
              .left {
                .imgs {
                  // border:1px solid @base_color;
                }
                .title{
                  bottom: -50px;
                }
                .text_show{
                  top: 0;
                }
              }
            }
            &:nth-child(3n) {
              margin-right: 0px!important;
            }
            .left {
                width: 100%;
                height: 300px;
                position: relative;
                overflow: hidden;
                .imgs {
                    width: 100%;
                    display: inline-block;
                    border-radius: 5px;
                    overflow: hidden;
                    // border: 1px solid #eee;
                    transition: all .3s;
                    img {
                            width: 100%;
    height: 300px;
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
                    height: 50px;
                    text-align: center;
                    background-color: rgba(0, 0, 0, 0.6);
                    -webkit-transition: all 0.3s;
                    transition: all 0.3s;
                    p{
                      font-size: 18px;
                      color: #fff;
                      line-height: 50px;
                    }
                }
                .text_show{
                  position: absolute;
                  top: -300px;
                  left: 0;
                  width: 100%;
                  height: 300px;
                  background-color: rgba(222, 41, 16, 0.9);
                  -webkit-transition: all 0.3s;
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
              text-align: center;
            }
                }
            }
            .right {
                width: 100%;

                .contentTime {
                    font-size: 18px;
                    color: #999999;

                }
                 .desc {
                    font-size: 18px;
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
                    justify-content: flex-start;
                    margin-top: 1rem;
                    a {
                        font-size: 18px;
                        color: #b19162;
                        border-bottom: 1px solid #b19162;
                    }
                }

            }
        }
}
</style>
