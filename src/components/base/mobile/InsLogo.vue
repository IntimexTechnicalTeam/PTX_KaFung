<template>
  <a href="/" class="logo">
    <img src="/images/pc/pcindex_09.png" />
  </a>
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
    width: 22rem;
    display: block;
  }
}
</style>
