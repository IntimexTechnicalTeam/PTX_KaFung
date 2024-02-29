<template>
  <div class="map-main fix">
    <div class="body fix" v-html="Content.Body"></div>
  </div>
</template>
<script lang="ts" scoped>
import { Component, Prop, Vue } from 'vue-property-decorator';
@Component
export default class PkMap extends Vue {
  Content:string='';

  getContent () {
    this.$Api.cms.getContentByDevice({ key: 'contact', IsMobile: false }).then(result => {
      this.Content = result.CMS;
    });
  }
  mounted() {
    this.getContent();
  }
}
</script>

<style scoped lang="less">
.map-main {
  width: 1200px;
  margin: 0 auto;
  .body{
    margin: 0 100px;
    /deep/ .left{
      float: left;
    }
    /deep/ .right{
      float: left;
      margin-left: 30px;
      p{
        font-size: 16px;
        margin-bottom: 15px;
        span{
          font-size: 16px;
          color: #288dc9;
        }
      }
    }
  }
}
.map-main img {
  width: 100%;
}
</style>
