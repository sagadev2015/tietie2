<template>
  <div style="padding-bottom:10px;margin-top:-58px" id="bottomDialog">
  
    <el-button type="primary" size="small" style="position:absolute;left:200px;top:10px;" @click="toggle()" v-text="btnText" />
    <div class="bottom-s-c">
      <class-history v-if="view1" :sub-category-id="subCategoryId" :shop-id="shopId"/>
      <store-history v-else :shop-id="shopId" />
    </div>

  </div>
</template>


<style>
.bottom-s-c {
  margin-top: 20px;
}
  @media screen and (max-width: 720px) {
    #bottomDialog {
      margin-top: 0px;
    }
  }
</style>
<script>
import StoreHistory from './StoreHistory.vue';
import ClassHistory from './ClassHistory.vue';

export default {
  components: { StoreHistory, ClassHistory },
  props: {
    subCategoryId: {
      type: Number,
      default: 0,
    },
    shopId: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      view1: true,
      btnText: '店舗過去履歴',
    };
  },
  mounted() {
    this.$route.params.accounting_ym = false;
    //pop up queryselect from bottom to up
    
    // const dialogs = document.querySelectorAll('.bottom-dialog');
    // dialogs.forEach(el => {
    //   el.closest('.el-dialog__wrapper').classList.add('bottom-dialog-wrapper');
    // });
  },
  methods: {
    toggle(){
      this.view1 = !this.view1;
      if (this.view1){
        this.btnText = '店舗過去履歴';
      } else {
        this.btnText = '同一中分類過去履歴';
      }
    },
  },
};
</script>
