<template>
  <div class="app-container">
    <div class="filter-container">
      <el-input
        v-model="searchParams.itemId"
        class="filter-item"
        style="width: 200px"
        placeholder="请输入 itemID"
        @keyup.enter.native="handlerFilter"/>
      <el-input
        v-model="searchParams.skuId"
        class="filter-item"
        style="width: 200px"
        placeholder="请输入 skuID"
        @keyup.enter.native="handlerFilter"/>
      <el-input
        v-model="searchParams.productName"
        class="filter-item"
        style="width: 200px"
        placeholder="请输入商品名称"
        @keyup.enter.native="handlerFilter"/>
      <el-button
        v-waves
        class="filter-item"
        type="primary"
        icon="el-icon-search"
        @click="handlerFilter"
      >搜索</el-button>
      <el-button
        v-waves
        class="filter-item"
        type="primary"
        icon="el-icon-edit"
        @click="handleCreateItem"
      >添加</el-button>
    </div>
    <el-table
      v-loading="isLoading"
      :data="list"
      border>
      <el-table-column
        :label="tableCellLabel.itemId"
        align="center"
        width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.itemId }}</span>
        </template>
      </el-table-column>
      <el-table-column
        :label="tableCellLabel.SKUId"
        align="center"
        width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.skuId }}</span>
        </template>
      </el-table-column>
      <el-table-column
        :label="tableCellLabel.productName"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.productName }}</span>
        </template>
      </el-table-column>
      <el-table-column
        :label="tableCellLabel.productCode"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.productCode }}</span>
        </template>
      </el-table-column>
      <el-table-column
        :label="tableCellLabel.brand"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.brand }}</span>
        </template>
      </el-table-column>
      <el-table-column
        :label="tableCellLabel.category"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.category }}</span>
        </template>
      </el-table-column>
      <el-table-column
        :label="tableCellLabel.specification"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.specification }}</span>
        </template>
      </el-table-column>
      <el-table-column
        :label="tableCellLabel.salePrice"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.salePrice }}</span>
        </template>
      </el-table-column>
      <el-table-column
        :label="tableCellLabel.stock"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.stock }}</span>
        </template>
      </el-table-column>
      <el-table-column
        :label="tableCellLabel.isPutAway"
        class-name="status-col"
        width="100">
        <template slot-scope="scope">
          <el-tag
            :type="scope.row.isPutAway ? 'success' : 'danger'">
            {{ scope.row.isPutAway ? '上架' : '下架' }}
          </el-tag>
        </template>
      </el-table-column>
      <el-table-column
        label="操作"
        align="center"
        width="230"
        class-name="small-padding fixed-width">
        <template slot-scope="scope">
          <el-button type="primary" size="mini" @click="handleEditItem(scope.row)">编辑</el-button>
          <el-button :type="!scope.row.isPutAway ? 'success' : 'danger'" size="mini" @click="handleModifyPutAway(scope.row)">{{ scope.row.isPutAway ? '下架' : '上架' }}
          </el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- .sync 双向绑定 dialogVisible -->
    <el-dialog
      :title="dialogStatus && dialogMap[dialogStatus].title"
      :visible="dialogVisible">
      <el-form
        ref="dataForm"
        :model="item"
        :rules="dialogStatus && dialogMap[dialogStatus].rules"
        label-width="70px"
        style="width: 400px;">
        <el-form-item
          :label="tableCellLabel.itemId"
          prop="itemId">
          <el-input :disabled="true" :placeholder="item.itemId" class="filter-item"/>
        </el-form-item>
        <el-form-item
          :label="tableCellLabel.SKUId"
          prop="SKUId">
          <el-input :disabled="true" :placeholder="item.SKUId" class="filter-item"/>
        </el-form-item>
        <el-form-item
          :label="tableCellLabel.productName"
          prop="productName">
          <el-input :placeholder="item.productName" class="filter-item"/>
        </el-form-item>
        <el-form-item
          :label="tableCellLabel.productCode"
          prop="productCode">
          <el-input :placeholder="item.productCode" class="filter-item"/>
        </el-form-item>
        <el-form-item
          :label="tableCellLabel.brand"
          prop="brand">
          <el-input :placeholder="item.brand" class="filter-item"/>
        </el-form-item>
        <el-form-item
          :label="tableCellLabel.category"
          prop="category">
          <el-input :placeholder="item.category" class="filter-item"/>
        </el-form-item>
        <el-form-item
          :label="tableCellLabel.specification"
          prop="specification">
          <el-input :placeholder="item.specification" class="filter-item"/>
        </el-form-item>
        <el-form-item
          :label="tableCellLabel.salePrice"
          prop="salePrice">
          <el-input :placeholder="item.salePrice" class="filter-item"/>
        </el-form-item>
        <el-form-item
          :label="tableCellLabel.stock"
          prop="stock">
          <el-input :placeholder="item.stock" class="filter-item"/>
        </el-form-item>
        <el-form-item
          :label="tableCellLabel.isPutAway"
          prop="isPutAway">
          <el-input :placeholder="item.isPutAway" class="filter-item"/>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button
          @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="dialogStatus === 'add' ? confirmAdd() : confirmEdit()">提交</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import waves from '@/directive/waves'
export default {
  name: 'ItemManage',
  directives: { waves },
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'info',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      // 基本配置
      dialogMap: {
        add: {
          title: '添加商品',
          rules: {} // 输入规则
        },
        edit: {
          title: '编辑商品',
          rules: {} // 输入规则
        }
      },
      tableCellLabel: {
        itemId: 'itemId',
        SKUId: 'SKUId',
        productName: '商品名称',
        productCode: '商品条码',
        brand: '品牌',
        category: '类目',
        specification: '规格',
        salePrice: '售价',
        stock: '库存',
        isPutAway: '上下架'
      },
      isLoading: false,
      list: null,
      searchParams: { // 搜索参数
        itemId: '',
        skuId: '',
        productName: ''
      },
      dialogStatus: '',
      dialogTitle: '',
      dialogVisible: false,
      rules: {}, // 输入规则
      item: { // 添加编辑 item
        itemId: '',
        skuId: '',
        productName: '',
        productCode: '',
        brand: '',
        category: '',
        specification: '',
        salePrice: '',
        stock: '',
        isPutAway: false
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
        itemId: 1,
        skuId: 2,
        productName: 'test',
        productCode: '123456',
        brand: '牛人',
        category: '宇宙',
        specification: 'red',
        salePrice: 2,
        stock: 20,
        isPutAway: true
      }]
    },
    handlerFilter() {
      // search api
    },
    handleCreateItem() {
      this.dialogStatus = 'add'
      this.dialogVisible = !this.dialogVisible
    },
    handleEditItem(item) {
      this.dialogStatus = 'edit'
      this.dialogVisible = !this.dialogVisible
    },
    confirmEdit() {
      // edit api
    },
    confirmAdd() {
      // add api
    },
    handleModifyPutAway() {
      // 修改上下架
    },
    handleEdit() {
      // 重新编辑
    }
  }
}
</script>
