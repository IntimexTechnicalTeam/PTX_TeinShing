<template>
    <li class="NsReSearchItem">
        <p class="category">
          {{searchGroup.Name}}
            <i class="el-icon-arrow-down colorstyle"  @click="isOpen = !isOpen" v-if="!isOpen"></i>
            <i class="el-icon-arrow-up" @click="isOpen = !isOpen" v-else></i>
        </p>
        <transition name="fade">
        <ul class="open" :class="{'show': isOpen}">
            <li class="viewall" :class="{'borderLine':isAll}">
                <input type="checkbox" :id="searchGroup.Name+'-All'" v-model="isAll" @click="checkAll($event,searchGroup)" style="display:none;">
                <label :for="searchGroup.Name+'-All'" >{{$t('Message.All')}}</label>
            </li>
            <li v-for="(child, index2) in (searchType === 1 ? searchGroup.AttrValues : searchType === 2 ? searchGroup.Children : [])" :key="index2">
                <input style="display:none;" type="checkbox" :id="child.Name+index2" :value="child.Id" v-model="checkedValue" @click="selectAttr(searchGroup)">
                <label :for="child.Name+index2"  @click="checkLi(index2)">{{child.Name}}</label>
            </li>
            <!-- <i class="el-icon-plus" @click="isOpen = !isOpen" v-if="!isOpen"></i>
            <i class="el-icon-minus" @click="isOpen = !isOpen" v-else></i> -->
        </ul>
        </transition>
    </li>
</template>
<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
export interface attrItem {
  Id: number,
  Vals: number[]
}
@Component
export default class NsNsReSearchItem extends Vue {
    @Prop({ default: () => {} }) private searchGroup!: any;
    @Prop({ default: () => [] }) private defaultSelected!: number[];
    @Prop({ default: 1 }) private searchType!: number; // 搜索数据类型（1 => 产品属性， 2 => 产品目录）

    isOpen: boolean = false; // 是否展开
    isAll: boolean = false; // 是否全选
    checkedValue: number[] = []; // 选中的产品属性值
    selectedAttrs: attrItem[] = []; // 选中的产品属性值
    showSubmeun () {
      this.isOpen = !this.isOpen;
    }
    checkLi (val) {
      $('.toggleLi' + val).toggleClass('borderLine');
    }
    //  全选（产品属性）
    checkAll (e, attr) {
      console.log('checkAll');
      let a = e.target.checked;
      if (e.target.checked) {
        this.checkedValue = [];
        if (this.searchType === 1) {
          attr.AttrValues.forEach(element => {
            this.checkedValue.push(element.Id);
          });
          this.$emit('changeSelect', attr.Id, this.checkedValue);
        } else if (this.searchType === 2) {
          console.log('产品目录全选');
          attr.Children.forEach(element => {
            this.checkedValue.push(element.Id);
          });
          this.$emit('changeSelect', attr.Id, this.checkedValue);
        }
      } else {
        this.checkedValue = [];
        this.$emit('changeSelect', attr.Id, this.checkedValue);
      }
    }

    //  单属性选择（产品属性）
    selectAttr (item) {
      console.log(item, '单属性选择（产品属性）');
      setTimeout(() => {
        if (this.searchType === 1) {
          if (this.checkedValue.length !== this.searchGroup.AttrValues.length) {
            this.isAll = false;
          } else {
            this.isAll = true;
          }
          this.$emit('changeSelect', item.Id, this.checkedValue);
        } else if (this.searchType === 2) {
          console.log(item.Id, '改变的产品目录id');
          if (this.checkedValue.length !== this.searchGroup.Children.length) {
            this.isAll = false;
          } else {
            this.isAll = true;
          }
          this.$emit('changeSelect', item.Id, this.checkedValue);
        }
      }, 1);
    }

    created () {
      if (this.defaultSelected.length) {
        if (this.searchType === 1) {
          if (this.defaultSelected.length === this.searchGroup.AttrValues.length) {
            this.isAll = true;
          }
        } else if (this.searchType === 2) {
          if (this.defaultSelected.length === this.searchGroup.Children.length) {
            this.isAll = true;
          }
        }
        this.checkedValue = this.defaultSelected;
      }
    }
}
</script>
<style scoped lang="less">
.NsReSearchItem {
      .colorstyle {
        color: @base_color;
      }
      .borderLine {
        // border: 1px solid @base_color;
        color: #de2910;
        &::before{
          content: '';
          width: 6px;
          height: 100%;
          background-color: #de2910;
          position: absolute;
          left: 0;
          top: 0;
        }
      }
      .viewall {
        border-radius: .5rem;
        background: #f5f5f5;
        margin-top: 2rem;
        overflow: hidden;
        label {
          // color: @base_color!important;
          color: #808080;
          text-align: center;
        }
      }
      p.category {
        // width: 100%;
        font-size: 1.4rem;
        color: #fff;
        // margin-right: 1rem;
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        justify-self: start;
        align-items: center;
        -ms-flex-negative: 0;
        flex-shrink: 0;
        position: relative;
        height: 3.5rem;
        line-height: 3.5rem;
        border-radius: 2rem;
        // padding-top: 1rem;
        background-color: #de2910;
        justify-content: center;
        margin-left: 5%;
            margin-right: 5%;
            box-sizing: border-box;
        i{
          // position: absolute;
          // right: 0px;
          // top: 20px;
          font-size: 1.6rem;
          color: #fff;
          margin-left: 1rem;
        }
    }

     >ul {
       transition: all 3s;
       padding-bottom: 1rem;
        box-shadow: 0 2px 5px #d1d1d1;
      //  margin-bottom: 10rem;
        >li {
            height: 3.5rem;
            display: flex;
            align-items: center;
            justify-content: flex-start;
            background: #f5f5f5;
            border-radius: .3rem;
            margin-bottom: 1rem;
            // border:1px solid #f2f1f0;
            position: relative;
            overflow: hidden;
            margin-left: 5%;
            margin-right: 5%;
            box-sizing: border-box;
            input[type="checkbox"] {
                width: 18px;
                height: 18px;
                background-color: #fff;
                -webkit-appearance:none;
                border: 1px solid #cccccc;
                outline: none;
            }

            input[type="checkbox"]:checked {
                border: 1px solid #666666;
                background-image: url('/images/mobile/checked.png'); /*复选框的背景图*/
                background-repeat: no-repeat;
                background-position: center;
                background-size: auto;

                &+label {
                    color: #de2910;
                    text-align: center;
                    &::before{
                      content: '';
                      width: 6px;
                      height: 100%;
                      background-color: #de2910;
                      position: absolute;
                      left: 0;
                      top: 0;
                    }
                }
            }

            label {
            font-size: 1.4rem;
            color: #808080;
            width: 90%;
            margin: 0 auto;
            text-align: center;
            }
        }

        i {
            position: absolute;
            font-size: 26px;
            right: 18px;
            top: 18px;
        }

        &.open {
                display: none;
                transition: all 3s;
        }
        &.show{
          display: block;
          transition: all 3s;
        }
    }
}
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
