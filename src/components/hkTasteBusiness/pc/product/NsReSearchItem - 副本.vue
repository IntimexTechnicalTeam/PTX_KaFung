<template>
    <li class="NsSearchItem">
          <!-- <p class="category">
          {{searchGroup.Name}}
            <i class="el-icon-arrow-up"  @click="isOpen = !isOpen" v-if="!isOpen"></i>
            <i class="el-icon-arrow-down" @click="isOpen = !isOpen" v-else></i>
        </p> -->
          <transition name="fade">
          <ul :class="{'open': isOpen}">
            <li class="viewall">
                <input name="fade_group" type="radio" :id="searchGroup.Name+'-All'" v-model="searchGroup.Id" :value="searchGroup.Id" @click="checkAll($event,searchGroup)" style="display:none;">
                <label :for="searchGroup.Name+'-All'" >
                  <!-- {{$t('Message.All')}} -->
                  {{searchGroup.Name}}
                </label>
            </li>
            <li v-for="(child, index2) in (searchType === 1 ? searchGroup.AttrValues : searchType === 2 ? searchGroup.Children : [])" :key="index2">
                <input style="display:none;" type="checkbox" :id="child.Name+index2" :value="child.Id" v-model="checkedValue" @click="selectAttr(searchGroup)" >
                <label :for="child.Name+index2">{{child.Name}}</label>
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
    @Prop({ default: 0 }) private key!: number;

    isOpen: boolean = false; // 是否展开
    isAll: boolean = true; // 是否全选
    checkedValue: number[] = []; // 选中的产品属性值
    selectedAttrs: attrItem[] = []; // 选中的产品属性值
    // isCheck:number = -1;
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
          // alert(1);
          this.$emit('changeSelect', attr.Id, this.checkedValue);
        } else if (this.searchType === 2) {
          console.log('产品目录全选');
          // attr.Children.forEach(element => {
          //   this.checkedValue.push(element.Id);
          // });
          // alert(2);
          // alert(e.target.value);
          this.checkedValue = [e.target.value];
          this.$emit('changeSelect', attr.Id, this.checkedValue);
        }
      } else {
        // alert(3);
        this.checkedValue = [];
        // this.checkedValue.push(e.target.checked);
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
          // alert(this.defaultSelected[0] + 'vs' + this.searchGroup.Id);

          // if (this.defaultSelected.length === this.searchGroup.Children.length) {
          //   this.isAll = true;
          // } else if (this.defaultSelected[0] === this.searchGroup.Id) {
          //   this.isAll = true;
          // }
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
      .borderLine {
        border: 1px solid @base_color;
       }
      .viewall {
        border-radius: .5rem;
        background: #fff;
        margin-top: 0;
      }
      p.category {
        width: 100%;
        font-size: 20px;
        color: #333333;
        background-color: #FFF;
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        justify-self: start;
        -webkit-box-align: center;
        -ms-flex-align: center;
        align-items: center;
        -ms-flex-negative: 0;
        flex-shrink: 0;
        position: relative;
        height: 40px;
        // line-height: 40px;
        border-radius: .3rem;
        padding-top:10px;
        padding-bottom: 10px;
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
            // height: 45px;
            // display: flex;
            // align-items: center;
            // justify-content: flex-start;
            // background: #f2f1f0;
            // border-radius: .3rem;
            margin-bottom: 10px;
            // border:1px solid #f2f1f0;
            cursor: pointer;
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
                    color: @base_color;
                }
            }
            input[type="radio"] {
                opacity: 0;
            }

            input[type="radio"]:checked {
                // border: 1px solid @base_color;

                &+label {
                    color: @base_color;
                    border: 1px solid @base_color;
                    background: #fff;
                }
            }

            label {
            font-size: 16px;
            color: #666;
            width: 100%;
            margin: 0 auto;
            cursor: pointer;
            height: 45px;
            display: flex;
            align-items: center;
            justify-content: flex-start;
            background: #fff;
            border-radius: 0.3rem;
            // margin-bottom: 1rem;
            border: 1px solid #f2f1f0;
            padding: 0 10px;
            box-sizing: border-box;
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
    }
}
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
