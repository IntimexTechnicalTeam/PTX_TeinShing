<template>
    <div class="langSwitch">
        <!-- <p @click="changeLang('C')" :class="{'active': $Storage.get('locale') === 'C'}">繁體</p>|
        <p @click="changeLang('S')" :class="{'active': $Storage.get('locale') === 'S'}">简体</p>|
        <p @click="changeLang('E')" :class="{'active': $Storage.get('locale') === 'E'}">ENG</p> -->

      <!-- <p @click="changeLang(lang.value)" v-for="(lang,index) in this.$Settings.langList" :key="index" :class="{'active': $Storage.get('locale') === lang.value}">{{lang.name}}</p> -->
      <select  v-model="currentlang">
        <option :value="lang.value" v-for="(lang,index) in this.$Settings.langList" :key="index">{{lang.name}}</option>
      </select>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import Cookie from 'js-cookie';
@Component
export default class InsLangSwitch extends Vue {
  get currentlang () {
    return this.$i18n.locale;
  }
  set currentlang (val) {
    this.changeLang(val);
  }
  changeLang (lang) {
    this.$Api.member.setUILanguage(lang).then((result) => {
      this.$i18n.locale = lang;
      localStorage.setItem('locale', lang);
      this.Reload();
    }).catch((error) => {
      console.log(error);
    });
 }
}
</script>
<style scoped lang="less">
.langSwitch {
    color: #999;
    display: flex;
    align-items: center;
    justify-content: center;
    p {
      display: inline-block;
      padding: 0 10px;
      cursor: pointer;
      position: relative;
      color: #999;
      &:last-child {
        padding-right: 0;
      }
    }
    .active {
      color: #333;
    }
    select{
          width: 100%;
    font-size: 14px;
    background: none;
    color: #bbc8d0;
    border: none;
    outline: none;
    height: 30px;
    margin-left: 5px;
    text-align: center;
    cursor: pointer;
      outline: none;
      option{
        color:#bbc8d0;
        outline: none;
      }
    }
}
</style>
