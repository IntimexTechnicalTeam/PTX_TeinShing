<template>
<div class="logo">
  <a href="/" v-if="$Storage.get('locale') === 'E'">
    <img src="/images/mobile/Mlogoeng.png" />
  </a>
  <a href="/" v-else>
    <img src="/images/mobile/Mlogo.png" />
  </a>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
@Component
export default class InsLogo extends Vue {

}
</script>
<style scoped lang="less">
.logo {
  img {
    width: 100%;
    display: block;
  }
}
</style>
