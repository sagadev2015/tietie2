<template>
  <div>
    <!-- {{ detail.create_at }}
  {{ detail.photo_files.length }} -->
    <h3>見積情報</h3>
    <el-table
      :data="detail.quotation_info"
      :show-header="true"
      border
      style="width: 100%"
    >
      <el-table-column align="center" prop="date" label="受取日" :formatter="formatterDate" />
      <el-table-column
        align="center"
        prop="amount"
        label="金額"
        :formatter="formatterCurrency"
      />
      <el-table-column align="center" prop="comment" label="摘要" />
      <el-table-column
        align="center"
        prop="editor"
        label="入力者"
        width="100"
      />
    </el-table>
    <div style="margin: 5px 0">
      <el-button
        style="float: right; margin-left: 10px"
        type="primary"
        size="small"
        @click="editVisibleChange()"
        >見積登録</el-button
      >
      <!-- <el-button style="float: right; margin-left:10px;" type="primary" size="small" @click="createQuotation = true">見積登録</el-button> -->
      <el-button type="info" size="mini" @click="quotationFilesVisible = true"
        >見積書({{ this.$route.params['q_cnt'] }})</el-button
      >
      <el-button type="info" size="mini" @click="photoFilesVisible = true"
        >写真({{ this.$route.params['qp_cnt'] }})</el-button
      >
      <!-- <el-button type="info" size="mini" @click="reportFilesVisible = true"
        >報告書({{ this.$route.params['r_cnt'] }})</el-button
      > -->
    </div>

    <el-dialog
      title="【見積書ファイルリスト】"
      :visible.sync="quotationFilesVisible"
      :modal="false"
      :width="filedialogWidth"
    >
      <quotation-files :detail="detail" />
      <span slot="footer" class="dialog-footer">
        <el-button @click="quotationFilesVisible = false">閉じる</el-button>
      </span>
    </el-dialog>
    <el-dialog
      :modal="false"
      title="【見積書写真リスト】"
      :visible.sync="photoFilesVisible"
      :width="filedialogWidth"
    >
      <qphoto-files :detail="detail" />
      <span slot="footer" class="dialog-footer">
        <el-button @click="photoFilesVisible = false">閉じる</el-button>
      </span>
    </el-dialog>

    <!-- <el-dialog
      title="【報告書ファイルリスト】"
      :visible.sync="reportFilesVisible"
      :width="filedialogWidth"
      :modal="false"
    >
      <report-files :detail="detail" />
      <span slot="footer" class="dialog-footer">
        <el-button @click="reportFilesVisible = false">閉じる</el-button>
      </span>
    </el-dialog> -->

    <el-dialog
      title="見積情報 登録"
      :visible.sync="editVisible"
      :width="editdialogWidth"
      custom-class="slide-dialog"
      top="0px"
      :modal="false"
    >
      <create-quotation :detail="detail" @create="editVisible = false" />
    </el-dialog>
  </div>
</template>

<script>
import QuotationFiles from './sub/QuotationFiles.vue';
import PhotoFiles from './sub/PhotoFiles.vue';
import QphotoFiles from './sub/QphotoFiles.vue';
import ReportFiles from './sub/ReportFiles.vue';
import ProgressEdit from './sub/ProgressEdit.vue';

import AccountingInfo from './AccountingInfo.vue';
import QuotationInfo from './QuotationInfo.vue';
import CreateQuotation from './sub/CreateQuotation.vue';
import Resource from '@/api/resource';

export default {
  components: {
    QuotationFiles,
    QphotoFiles,
    PhotoFiles,
    ProgressEdit,
    ReportFiles,
    QuotationInfo,
    AccountingInfo,
    CreateQuotation,
  },
  props: {
    detail: {
      type: Object,
      default: () => {
        return {};
      },
    },
  },
  data() {
    return {
      editVisible: false,
      quotationFilesVisible: false,
      photoFilesVisible: false,
      reportFilesVisible: false,
      progress: [],
      q_cnt: 0,
      r_cnt: 0,
      p_cnt: 0,
      filedialogWidth: '700px',
      editdialogWidth: '45%',
    };
  },
  created() {
    this.progress = {
      1: 'BM承認待ち',
      2: 'BM承認',
      3: 'BM差戻し',
      4: 'BM却下',
      5: 'BM保留',
      6: '本部受付前',
      7: '本部差戻し',
      8: '本部見送り',
      9: '依頼確定',
      10: '依頼済',
      11: '見積待ち',
      12: '見積精査中',
      13: '入荷待ち',
      14: 'DM承認待ち',
      15: '稟議中',
      16: '見積発注済み',
      17: '日程調整中',
      18: '訪問待ち',
      19: '報告待ち',
      20: '店完了',
      21: '取完了',
      22: '問合せ中',
    };
  },
  mounted() {
    const dialogs = document.querySelectorAll('.slide-dialog');
    dialogs.forEach((el) => {
      el.closest('.el-dialog__wrapper').classList.add('slide-dialog-wrapper');
    });

    if(this.isMobile()) {
      this.filedialogWidth = '100%';
      this.editdialogWidth = '100%';
    }

  },
  methods: {
    formatterDate(row, column){
      var date = row.date;
      var date_arr;
      if(date != null && date != ''){
        date_arr = date.split('-');
        return date_arr[0] + '/' + date_arr[1] + '/' + date_arr[2];
      }

      return;
    },
    isMobile() {
      var check = true;
      if(document.querySelector("body").clientWidth > 737) check = false;
      return check;
    },

    editVisibleChange() {
      this.editVisible = true;
    },
    formatterProgress(row, column) {
      return this.progress[row.progress_id] ?? '';
    },

    formatterCurrency(row, column) {
      if (row.amount == null) return;
      return `¥ ${row.amount }`.replace(/\B(?=(\d{3})+(?!\d))/g, ',');
    },

  },
};
</script>