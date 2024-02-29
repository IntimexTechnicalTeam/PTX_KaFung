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
    this.$Api.cms.getContentByDevice({ key: 'contact', IsMobile: true }).then(result => {
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
  width: 90%;
  margin: 0 auto;
  // display: flex;
  margin-bottom: 3rem;
  .body{
    text-align: center;
    /deep/ .left{
      text-align: center;
    }
    /deep/ .right{
      text-align: center;
      p{
        font-size: 1.2rem;
        margin-bottom: 1rem;
        // display: flex;
        // justify-content: center;
        span{
          font-size: 1.2rem;
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
