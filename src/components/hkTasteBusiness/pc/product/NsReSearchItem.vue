<template>
    <li class="NsSearchItem" :class="{'ENG':$Storage.get('locale') === 'E'}">
      <div class="title">
        <p class="category">
          {{searchGroup.Name}}
            <!-- <i class="el-icon-arrow-up"  @click="isOpen = !isOpen" v-if="!isOpen"></i>
            <i class="el-icon-arrow-down" @click="isOpen = !isOpen" v-else></i> -->
        </p>
      </div>

          <transition name="fade">
          <ul :class="{'open': isOpen}">
            <li class="viewall" :class="{'borderLine':isAll}">
                <input type="checkbox" :id="searchGroup.Name+'-All'" v-model="isAll" @click="checkAll($event,searchGroup)"  style="display:none;">
                <label :for="searchGroup.Name+'-All'" >{{$t('Message.All')}}</label>
            </li>
            <li v-for="(child, index2) in (searchType === 1 ? searchGroup.AttrValues : searchType === 2 ? searchGroup.Children : [])" :key="index2">
                <input type="checkbox" :id="child.Name+index2" :value="child.Id" v-model="checkedValue" @click="selectAttr(searchGroup)">
                <label :for="child.Name+index2" @click="checkLi(index2)">{{child.Name}}</label>
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
export default class InsAdvancedSearch extends Vue {
    @Prop({ default: () => {} }) private searchGroup!: any;
    @Prop({ default: () => [] }) private defaultSelected!: number[];
    @Prop({ default: 1 }) private searchType!: number; // 搜索数据类型（1 => 产品属性， 2 => 产品目录）

    isOpen: boolean = false; // 是否展开
    isAll: boolean = false; // 是否全选
    checkedValue: number[] = []; // 选中的产品属性值
    selectedAttrs: attrItem[] = []; // 选中的产品属性值
    isCheck:number = -1;
    //  全选（产品属性）
    checkLi (val) {
      $('.toggleLi' + val).toggleClass('borderLine');
    }
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
.NsSearchItem {
      .colorstyle {
        color: @base_color;
      }

      .viewall {
        // border-radius: .5rem;
        background: #f5f5f5;
        // margin-top: 1rem;
      }
      .title{
        width: 100%;
        height: 70px;
        background: url('/images/pc/producttitle.png') no-repeat;
        text-align: center;
        margin-bottom: 20px;
      }
      p.category {
        font-size: 20px;
        color: #fff;
        position: relative;
        line-height: 66px;
        font-weight: bold;
        padding-left: 20px;
        i{
          position: absolute;
          right: 0px;
          top: 20px;
          font-size: 1.6rem;
          cursor: pointer;
        }
    }

     >ul {
       transition: all 3s;
        >li {
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: flex-start;
            background: #f5f5f5;
            border-radius: 3px;
            margin-bottom: 10px;
            // border:1px solid #f2f1f0;
            cursor: pointer;
            position: relative;
            overflow: hidden;
            input[type="checkbox"] {
                // width: 18px;
                // height: 18px;
                background-color: transparent;
                -webkit-appearance:none;
                border: none;
                outline: none;
                position: absolute;
                left: 0;
                top: 0;
            }

            input[type="checkbox"]:checked {
                width: 6px;
                  height: 50px;
                  background-color: #de2910;
                  border-top-left-radius: 3px;
                  border-bottom-left-radius: 3px;
                  margin: 0;

                &+label {
                    color: #de2910;

                }
            }

            label {
            font-size: 18px;
            color: #808080;
            width: 90%;
            margin: 0 auto;
            cursor: pointer;
            height: 50px;
            line-height: 50px;
            text-align: center;
            overflow: hidden;
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
        .borderLine{
        label{
          color: #de2910 !important;
        }
        &::before {
        content: '';
        width: 6px;
        height: 50px;
        background-color: #de2910;
        position: absolute;
        left: 0;
        top: 0;
        border-top-left-radius: 3px;
        border-bottom-left-radius: 3px;
       }
      }
    }
}
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
.NsSearchItem.ENG{
  >ul>li {
    label{
      font-size: 16px;
      line-height: normal;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  }
}
</style>
