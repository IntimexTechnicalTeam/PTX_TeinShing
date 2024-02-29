<template>
  <div :style="styla" v-if="ShowItemsLength>0" class="PcVersionYouLike">
    <p class="NoramlTitle"><span class="text">{{title}}</span></p>
    <swiper :options="SwiperOption" ref="mySwiper">
      <!-- slides -->
      <swiperSlide v-for="(page,idx) in ShowItems" :key="idx">
        <div class="in_slider_page_container" @click="click">
          <div class="in_slider_page_item" v-for="(item,index) in page" :key="index">
            <div class="in_slider_page_item" v-if="!item.virtual">
              <inProductWindow :item="item" :imgStyla="imgStyla" styla="width:90%;margin:0 auto;"></inProductWindow>
            </div>
          </div>
        </div>
      </swiperSlide>
      <div class="swiper-button-prev" slot="button-prev"></div>
      <div class="swiper-button-next" slot="button-next"></div>
    </swiper>
  </div>
</template>
<script lang="ts">
import YouWouldLike from '@/model/youWouldLike';
import inButton from '@/components/base/pc/InsButton.vue';
import inProductWindow from '@/components/hkTasteBusiness/pc/product/HkProductWindow.vue';
import Currency from '@/model/currency';
import { Vue, Prop, Component, Watch } from 'vue-property-decorator';
import { swiper, swiperSlide } from 'vue-awesome-swiper/src';
@Component({
  components: {
    inButton, inProductWindow, swiper, swiperSlide
  }
})
export default class InsYouWouldLike extends Vue {
  // data
  private SwiperOption = {
    navigation: {
      nextEl: '.swiper-button-next',
      prevEl: '.swiper-button-prev'
    }
  };
  private InnerItems: YouWouldLike[] = [];
  private InnerItemsCopy: YouWouldLike[] = [];
  private ShowItems: YouWouldLike[][] = [];
  private Layer: boolean = false;
  private ShowItemsLength: number = 0;
  //   props
  @Prop() private readonly styla!: string;
  @Prop() private readonly imgStyla!: string;
  @Prop() private readonly title!: string;
  @Prop() private readonly pageNum!: number;
  // @Prop() private readonly items!: YouWouldLike[];
  @Prop() private readonly ProductSku!: string;
  //   method
  click (e: MouseEvent) {
    let target = e.target as HTMLElement;
    if (target.nodeName === 'IMG') {
    }
  }
  buttonClick (item: YouWouldLike) {
    console.log(item);
  }
  created () {
    this.$Api.product.getRltProduct(this.ProductSku).then((result) => { this.InnerItems = result.YouWouldLike; this.ShowItemsLength = result.YouWouldLike.length; });
  }
  @Watch('InnerItems')
  onInnerItemsChange (o, n) {
    this.InnerItemsCopy = this.InnerItems.slice();
    this.ShowItems.splice(0, this.ShowItems.length);
    while (this.InnerItemsCopy.length > 0) {
      this.ShowItems.push(this.InnerItemsCopy.splice(0, this.pageNum));
    }
    while (
      this.ShowItems.length > 0 &&
      this.ShowItems[this.ShowItems.length - 1].length < this.pageNum
    ) {
      this.ShowItems[this.ShowItems.length - 1].push(
        new YouWouldLike('-1', '', '', '', '', '', new Currency(), '', '', new Currency(), true)
      );
    }
  }
  get lang () {
    return this.$Storage.get('locale');
  }
  @Watch('ProductSku')
  onProductSkuChange (o, n) {
    this.$Api.product.getRltProduct(this.ProductSku).then(result => {
      this.InnerItems = result.YouWouldLike;
      this.ShowItemsLength = result.YouWouldLike.length;
    });
  }
}
</script>
<style lang="less">
.PcVersionYouLike .swiper-button-prev{
      width: 40px;
      height: 40px;
      border-top-right-radius: 5px;
      border-bottom-right-radius: 5px;
      // border:1px solid #c4a982;
      background: url('/images/mobile/other_31.png') no-repeat center center!important;
      background-size: 15px;
      outline: 0;
      left:.9rem;
}
.PcVersionYouLike .swiper-button-next{
      width: 40px;
      height: 40px;
      border-top-left-radius: 5px;
      border-bottom-left-radius: 5px;
      // border:1px solid #c4a982;
      background:  url('/images/mobile/other_32.png') no-repeat center center!important;
      background-size: 15px;
      outline: 0;
      right: .9rem;
}
</style>
<style  lang="less"  scoped>
  .NoramlTitle {
    height: 100px;
      background: url(/images/pc/index_21.png) no-repeat center center;
      position: relative;
      text-align: center;
      margin-bottom: 30px;
      &::before{
          content: '';
          width: 502px;
          height: 1px;
          background-color: #e3e6e8;
          position: absolute;
          left: 0;
          top: 50%;
          transform: translateY(-50%);
        }
        &::after{
          content: '';
          width: 502px;
          height: 1px;
          background-color: #e3e6e8;
          position: absolute;
          right: 0;
          top: 50%;
          transform: translateY(-50%);
        }
      .text{
        line-height: 100px;
        font-size: 32px;
        font-weight: bold;
        color: #2f4858;
        position: relative;
        letter-spacing: 2px;
      }
  }
.PcVersionYouLike {
  margin-top: 80px;
}
.in_slider_title {
  text-align: center;
  margin: 4rem 0;
  font-size: 2rem;
}
.in_slider_page_container {
  box-sizing: border-box;
  display: flex;
  flex-wrap: nowrap;
  width: 100%;
  margin: 0 auto;
  user-select: none;
  padding-top: 10px;
  padding-bottom: 10px;
}
.in_slider_page_item {
   width: 100%;

   /deep/ .in_pdWindow_page_item{
      border: 1px solid #f0f0f0;
      border-radius: 3px;
      &:hover{
        border: 1px solid #de2910;
        .in_pdWindow_item_description{
          background-color: #de2910;
          .in_pdWindow_item_title{
            color: #fff;
          }
        }
      }
   }
}
    .titleCn {
      width: 100%;
      display: inline-block;
      text-align: center;
      margin-bottom: 2rem;
      span {
        font-size:1.7rem;
        display: block;
        width: 60%;
        text-align: center;
        margin: 0 auto;
        text-transform: uppercase;
        font-weight: 700;
        color: #37aca1;
        &::before {
          content: '';
          border-top: 4px solid #37aca1;
          width: 15%;
          display: block;
          margin: 0 auto;
          margin-bottom: .5rem;
        }
        &::after {
          content: '';
          border-bottom:4px solid #37aca1;
          width: 15%;
          display: block;
          margin: 0 auto;
          margin-top: .5rem;
        }
      }
    }
.titleNameA {
  width: 100%;
  display: inline-block;
  text-align: center;
  margin-bottom: 2rem;
  span {
    font-size:32px;
    display: block;
    width: 60%;
    text-align: center;
    margin: 0 auto;
    text-transform: uppercase;
    font-weight: 700;
    color: #37aca1;
    &::before {
      content: '';
      border-top: 4px solid #37aca1;
      width: 35%;
      display: block;
      margin: 0 auto;
      margin-bottom: .5rem;
    }
    &::after {
      content: '';
      border-bottom:4px solid #37aca1;
      width: 35%;
      display: block;
      margin: 0 auto;
      margin-top: .5rem;
    }
  }
}
</style>
