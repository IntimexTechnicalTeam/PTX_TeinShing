<template>
  <div id="container" class="catDetail" :class="{'ENG':$Storage.get('locale') === 'E'}">
    <transition name="slide">
      <div key="1" v-if="!waiting">
        <div class="BannerImg">
            <img :src="BannerImg" v-show="BannerImg!==null">
        </div>
        <div class="catContent">
            <ins-cat-layout2 :catData="cmsCatTree" :cmsData="contentList" @changeCatSelect="changeCatSelect" v-if="cmsCategory.PageStyle === '0' || cmsCategory.PageStyle === '1'" />

            <ins-cat-layout1 v-if="cmsCategory.PageStyle === '2'"/>

            <ins-cat-layout3 :cmsData="contentList" v-if="cmsCategory.PageStyle === '3'" />

            <ins-cat-layout4 :cmsData="contentList" :tabData="TabData" :pager="pager" v-if="cmsCategory.PageStyle === '4'" />
        </div>
      </div>
    </transition>
    <transition name="slide">
        <div class="faker" key="2" v-if="waiting" v-loading="true"></div>
    </transition>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
@Component({
  components: {
    // InsBanner: () => import('@/components/base/InsBanner.vue'),
    InsCatLayout1: () => import('@/components/business/mobile/cms/InsCatLayout1.vue'),
    InsCatLayout2: () => import('@/components/business/mobile/cms/InsCatLayout2.vue'),
    InsCatLayout3: () => import('@/components/business/mobile/cms/InsCatLayout3.vue'),
    InsCatLayout4: () => import('@/components/business/mobile/cms/InsCatLayout4.vue')
  }
})
export default class insNews extends Vue {
    cmsCategory: object = {}; // cms下单个目录的信息
    cmsCatTree: object[] = []; // cms目录
    contentList: object[] = []; // cms内容列表
    catId: number = 0; // Tree点击获取内容列表的目录id
    PageStyle: string = '0'; // catDetail页面类型
    private waiting :boolean = true;
    BannerImg: string = '';
    TabData:any[]=[];
    pager: any = {
      currentPage: 1, // 当前页
      pageSize: 3, // 每页显示条目个数
      totalRecord: 0 // 总条目数
    }

    // 根据设备类型获取CMSCategory信息
    getCategoryByDevice () {
      this.$Api.cms.getCategoryByDevice({ CatId: this.id, IsMobile: true }).then((result) => {
        this.cmsCategory = result;
        this.PageStyle = result.PageStyle;
        setTimeout(() => {
          this.waiting = false;
        }, 2000);
        switch (result.PageStyle) {
            case '1':
              this.getContentsByCatId();
              break;
            case '2':
              this.getCategoryTree();
              break;
            case '3':
              this.getSubCatContents();
              break;
            case '4':
              this.getContentsByCatId();
              break;
        }
        this.getArgument(result.CatPaths[0].CatId);
        this.$nextTick(() => {
          if (result.Name) document.title = result.Name;
          (document.getElementsByName('keywords')[0] as any).content = result.SeoKeyword;
          (document.getElementsByName('description')[0] as any).content = result.SeoDesc;
          (document.getElementsByName('twitter:description')[0] as any).content = result.SeoDesc;
          (document.getElementsByName('twitter:title')[0] as any).content = result.Name;
        });
      }).catch((error) => {
        console.log(error, 'error');
        this.$message({
          message: error,
          type: 'error'
        });
      });
    }

    // 获取cms该id下所有的Category
    getCategoryTree () {
      this.$Api.cms.getCategoryTree({ id: this.id }).then((result) => {
        if (result && result.length) {
          this.cmsCatTree = result;
          this.catId = result[0].Id;
          this.getContentsByCatId();
        } else {
          this.getContentsByCatId();
        }
      });
    }

    // 获取cms该id下所有的cms
    getContentsByCatId () {
      let catId = this.catId || this.id;
      this.$Api.cms.getContentsByCatId(catId, this.pager.currentPage, this.pager.pageSize).then((result) => {
        // if (this.PageStyle === '4') {
        //   this.contentList = this.contentList.concat(result.Data);
        // } else {
        //   this.contentList = result.Data;
        // }
        this.contentList = result.Data;

        result.Data.forEach(function (i) {
          var newDate = new Date(i.CreateDate.replace(/-/g, '/'));
          i.CreateDate = newDate.getFullYear() + '-' + (newDate.getMonth() + 1) + '-' + newDate.getDate();
        });
        this.pager.totalRecord = result.TotalRecord;
      });
    }

    // 获取 Category 和所有 SubCategory 的 cms 列表
    getSubCatContents () {
      this.$Api.cms.getSubCatContents({ CatId: this.id, IsMobile: true }).then((result) => {
        console.log(result, ' 获取 Category 和所有 SubCategory 的 cms 列表');
        this.contentList = result.Data;
      });
    }

    getArgument(v) {
    this.$Api.cms.getCategoryByDevice({ CatId: v, IsMobile: true }).then(async (result) => {
      this.TabData = result;
      this.BannerImg = result.ImagePath;
    }).catch((error) => {
      console.log(error, 'error');
      this.$message({
        message: error,
        type: 'error'
      });
    });
  }

    // 树形控件选择的cms目录改变
    changeCatSelect (Id) {
      this.catId = Id;
      this.getContentsByCatId();
    }

    get id () {
      return this.$route.params.id;
    }

    mounted () {
      this.getCategoryByDevice();
    }

    @Watch('id', { deep: true })
    onKeyChange () {
      this.cmsCategory = {};
      this.cmsCatTree = [];
      this.contentList = [];
      this.catId = 0;
      this.getCategoryByDevice();
    }

    @Watch('pager.currentPage', { deep: true })
    onPagerChange() {
      this.getContentsByCatId();
    }
}
</script>
<style scoped lang="less">
.catDetail {
  .catContent {
    width: 100%;
    margin: 0 auto;
    .layer {
      font-size: 1.2rem;
    }
  }
  .BannerImg{
    width: 100%;
    height: 100%;
    img{
      width: 100%;
      display: block;
    }
  }
}
.faker{
  min-height: 26rem;
}
.catDetail.ENG{
  /deep/ .TabData ul li a{

  }
}
</style>
