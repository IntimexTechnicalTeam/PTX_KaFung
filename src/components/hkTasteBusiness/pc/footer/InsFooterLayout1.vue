<template>
<div id="footer">
  <div class="footbg">
    <div class="footerMain">
        <!-- <div class="footerTop">
            <p><span>whatsapp&nbsp;{{$t('home.Order')}}</span><b>6289 1789</b></p>
            <p><span>{{$t('home.TelSearch')}}</span><b>6289 1789</b></p>
        </div> -->
        <div class="footerBotttom">
          <div class="footerLeft">
              <ul v-for="(n,index) in footerMenus" :key="index">
                <li>
                    <a href="javascript:;" v-if="n.Type === 0" @click="toUrl(n)"><span>{{n.Name}}</span></a>
                    <router-link :to="To(n)"  v-else><span>{{n.Name}}</span></router-link>
                  <ul>
                    <li v-for="(c,index2) in n.Childs" :key="index2">
                       <a href="javascript:;" v-if="c.Type === 0" @click="toUrl(c)">
                              {{c.Name}}
                        </a>
                       <router-link :to="To(c)" v-else>{{c.Name}}</router-link>
                    </li>
                  </ul>
                </li>
             </ul>
          </div>
          <!-- <div class="footerRight">
            <p><img src="/images/pc/pcindex_14.png" /></p>
          </div> -->
          <div class="clear"></div>
           <p class="footercopy">
             <span>All rights reserved © {{currentYear}} Kai Fung Plastic Bags Co.    |    Powered by Eventizer
               <a href="https://eventizer.hk/" target="_blank">
               <img src="/images/pc/footerlogo.png">
               </a>
              </span>
             <!-- <span v-if="!isPtx">
                <p>{{$t('home.Weaccept')}}</p>
                <div>
                  <img src="/images/payment/stripe.png" />
                  <img src="/images/payment/WeChatPay.png" />
                  <img src="/images/payment/Alipay.png" />
                  <img src="/images/payment/PayMe.png" />
                  <img src="/images/payment/Paypal.png" />
                  <img src="/images/payment/MasterCard.png" />
                  <img src="/images/payment/VISA.png" />
                </div>
              </span> -->
            </p>
        </div>
    </div>
  </div>
</div>

</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class InsFooterLayout1 extends Vue {
  currentYear: number = 0;
  footerMenus: any[] = [];
    get isPtx () {
      if (localStorage.getItem('isPtx') === '0') {
        return false;
      } else {
        return true;
      }
  }
  goToTop () {
    let sTop = document.documentElement.scrollTop;
    let times = setInterval(() => {
      sTop -= 50;
      if (sTop <= 0) {
        document.documentElement.scrollTop = 0;
        clearInterval(times);
      } else {
        document.documentElement.scrollTop = sTop;
      }
    }, 1);
  }
  toUrl (n) {
    if (!n.IsNewWin && n.Url) {
      window.location.href = n.Url;
    } else if (n.IsNewWin && n.Url) {
      window.open(n.Url);
    }
  }
  To (n) {
    return n.Type === 1 ? '/cms/catDetail/' + n.Value.Id : n.Type === 2 ? '/CMS/content/' + n.Value.Id : n.Type === 3 ? '/RegNPay/Form/' + n.Value.Id : n.Type === 4 && !this.$store.state.catMenuType ? '/product/cat/' + n.Value.Id : n.Type === 4 && this.$store.state.catMenuType ? '/product/search/-?catalogs=' + JSON.stringify([parseInt(n.Value.Id)]) + '&type=0' : n.Type === 5 ? '/product/search/-?attrs=' + JSON.stringify([{ Id: parseInt(n.Value.Id), Vals: [] }]) + '&type=0' : '/product/search/-?attrs=' + JSON.stringify([{ Id: parseInt(n.ParentId), Vals: [parseInt(n.Value.Id)] }]) + '&type=0';
  }
  getMenu () {
    this.$Api.promotion.getMenu().then((result) => {
      this.footerMenus = result.ReturnValue.FooterMenus;
    });
  }
  created () {
    var date = new Date();
    this.currentYear = date.getFullYear();
    this.getMenu();
  }
}
</script>

<style scoped lang="less">
/* 底部文件 */
.footbg{
    // background: #9f2f34 url('/images/pc/pcindex_05.jpg') no-repeat center bottom;
    // background-size: cover;
    width: 1200px;
    margin: 0 auto;
    // display: inline-block;
    // padding-bottom: 10px;
    // min-height: 278px;
    background-color: #2687c6;
}
.footerMain{
    width: 1200px;
    margin: 30px auto 0;
    padding: 50px 0;
}
.footerTop{
    text-align: center;
    padding-top: 25px;
    padding-bottom: 25px;
    width: 100%;
}
.footerTop p{
    text-align: center;
    display: inline-block;
    margin-right: 50px;
}
.footerTop p span{
    font-size: 14px;
    color: #FFF;
    line-height: 35px;
    margin-right: 15px;
}
.footerTop p b{
    font-weight: 100;
    font-size: 35px;
    color: #FFF;
    line-height: 35px;
}
.footerBotttom{
    width: 100%;
}
.footerLeft{
    // float: left;
    width: 100%;
    display: flex;
    justify-content: center;
}
.footerLeft > ul{
    display: flex;
    justify-content: center;
}
.footerLeft > ul >li{
    // width: 100%;
    // line-height: 30px;
    border-right: 1px solid #FFF;
    float: left;
    padding-right: 15px;
    padding-left: 15px;
}
.footerLeft > ul:last-child >li{
  border-right: none;
}
.footerLeft > ul >li >a{
    font-size:16px;
    color:#FFF;
}
.footerLeft > ul >li >ul{
  width: 100%;
}
.footerLeft > ul >li >ul a{
    font-size: 16px;
    color:#FFF;
    display: inline-block;
    text-transform: uppercase;
}
.footerLeft > ul >li >ul a:hover{
   transform: translateY(-3px);
}
.footerLeft p{
    width: 100%;
    display: block;
    font-size: 14px;
    color: #fff;
    padding-top: 20px;
}
.footerLeft p img{
    display: inline-block;
    vertical-align: middle;
    padding-left: 10px;
}
.footerRight{
    float: right;
    width: 40%;
    text-align: center;
}
.footerRight img{
  width: 60%;
  display: block;
}
.footercopy{
  width: 100%;
  display: block;
  margin-top: 20px;
  text-align: center;
}
.footercopy span:nth-child(1){
  // float: left;
  color:#FFF;
  font-size: 14px;
}
.footercopy span:nth-child(1) img{
  display: inline-block;
  vertical-align:middle;
  padding-left: 10px;
  height: 35px;
}
.footercopy span:nth-child(2){
  float: right;
  width: 40%;
  // text-align: center;
  color:#FFF;
  font-size: 14px;
  display: flex;

  >p{
    flex-shrink: 0;
    margin-top: 10px;
    margin-right: 10px;
  }
}
.footercopy span:nth-child(2) img{
  display: inline-block;
  vertical-align:middle;
  margin: 5px;
  height: 32px;
}
</style>
