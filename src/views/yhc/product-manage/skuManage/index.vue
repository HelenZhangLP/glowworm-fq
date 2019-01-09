<template>
  <div class="app-container">
    <div class="filter-container">
      <ElInput
        v-model="searchParams.SKUId"
        class="filter-item"
        style="width: 200px"
        placeholder="请输入 SKUId"
        @keyup.enter.native="handlerFilter"
      />
      <ElInput
        v-model="searchParams.productName"
        class="filter-item"
        style="width: 200px"
        placeholder="请输入商品名称"
        @keyup.enter.native="handlerFilter"
      />
      <ElButton
        v-waves
        class="filter-item"
        type="primary"
        icon="el-icon-search"
        @click="handlerFilter"
      >
        搜索
      </ElButton>
    </div>
    <ElTable
      v-loading="isLoading"
      :data="list"
      border
    >
      <ElTableColumn
        :label="tableCellLabel.SKUId"
        align="center"
        width="100"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.skuId }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.productName"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.productName }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.productCode"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.productCode }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.brand"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.brand }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.category"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.category }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.specification"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.specification }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.updateTime"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.updateTime | parseTime('{y}-{m}-{d} {h}:{i}') }}</span>
        </template>
      </ElTableColumn>
    </ElTable>
    <!-- .sync 双向绑定 dialogVisible -->
    <!--<el-dialog-->
    <!--:title="dialogStatus && dialogMap[dialogStatus].title"-->
    <!--:visible="dialogVisible">-->
    <!--<el-form-->
    <!--ref="dataForm"-->
    <!--:model="item"-->
    <!--:rules="dialogStatus && dialogMap[dialogStatus].rules"-->
    <!--label-width="70px"-->
    <!--style="width: 400px;">-->
    <!--<el-form-item-->
    <!--:label="tableCellLabel.itemId"-->
    <!--prop="itemId">-->
    <!--<el-input :disabled="true" :placeholder="item.itemId" class="filter-item"/>-->
    <!--</el-form-item>-->
    <!--<el-form-item-->
    <!--:label="tableCellLabel.SKUId"-->
    <!--prop="SKUId">-->
    <!--<el-input :disabled="true" :placeholder="item.SKUId" class="filter-item"/>-->
    <!--</el-form-item>-->
    <!--<el-form-item-->
    <!--:label="tableCellLabel.productName"-->
    <!--prop="productName">-->
    <!--<el-input :placeholder="item.productName" class="filter-item"/>-->
    <!--</el-form-item>-->
    <!--<el-form-item-->
    <!--:label="tableCellLabel.productCode"-->
    <!--prop="productCode">-->
    <!--<el-input :placeholder="item.productCode" class="filter-item"/>-->
    <!--</el-form-item>-->
    <!--<el-form-item-->
    <!--:label="tableCellLabel.brand"-->
    <!--prop="brand">-->
    <!--<el-input :placeholder="item.brand" class="filter-item"/>-->
    <!--</el-form-item>-->
    <!--<el-form-item-->
    <!--:label="tableCellLabel.category"-->
    <!--prop="category">-->
    <!--<el-input :placeholder="item.category" class="filter-item"/>-->
    <!--</el-form-item>-->
    <!--<el-form-item-->
    <!--:label="tableCellLabel.specification"-->
    <!--prop="specification">-->
    <!--<el-input :placeholder="item.specification" class="filter-item"/>-->
    <!--</el-form-item>-->
    <!--<el-form-item-->
    <!--:label="tableCellLabel.salePrice"-->
    <!--prop="salePrice">-->
    <!--<el-input :placeholder="item.salePrice" class="filter-item"/>-->
    <!--</el-form-item>-->
    <!--<el-form-item-->
    <!--:label="tableCellLabel.stock"-->
    <!--prop="stock">-->
    <!--<el-input :placeholder="item.stock" class="filter-item"/>-->
    <!--</el-form-item>-->
    <!--<el-form-item-->
    <!--:label="tableCellLabel.isPutAway"-->
    <!--prop="isPutAway">-->
    <!--<el-input :placeholder="item.isPutAway" class="filter-item"/>-->
    <!--</el-form-item>-->
    <!--</el-form>-->
    <!--<div slot="footer" class="dialog-footer">-->
    <!--<el-button-->
    <!--@click="dialogVisible = false">取消</el-button>-->
    <!--<el-button type="primary" @click="dialogStatus === 'add' ? confirmAdd() : confirmEdit()">提交</el-button>-->
    <!--</div>-->
    <!--</el-dialog>-->
  </div>
</template>
<script>
import waves from '@/directive/waves'
export default {
  name: 'SKUManage',
  components: {},
  directives: { waves },
  data() {
    return {
      tableCellLabel: {
        SKUId: 'SKUId',
        productName: '商品名称',
        productCode: '商品条码',
        brand: '品牌',
        category: '类目',
        specification: '规格',
        updateTime: '更新时间'
      },
      list: [],
      isLoading: false,
      searchParams: {
        SKUId: '',
        productName: ''
      }
    }
  },
  created() {
    this.fetchList()
  },
  methods: {
    fetchList() {
      // fetchData
      this.list = [{
        skuId: '123',
        productName: '杏干',
        productCode: '1234567',
        brand: '好吃',
        category: '食品',
        specification: '大颗',
        updateTime: 1546584262841
      }]
    },
    handlerFilter() {
      // 搜索
    }
  }
}
</script>

<style scoped>

</style>
