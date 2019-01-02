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
    </div>
    <el-table
      v-loading="isLoading"
      :data="list"
      border>
      <el-table-column
        label="itemId"
        align="center"
        width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.itemId }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="skuId"
        align="center"
        width="100">
        <template slot-scope="scope">
          <span>{{ scope.row.skuId }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="商品名称"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.productName }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="商品条码"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.productCode }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="品牌"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.brand }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="类目"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.list }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="规格"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.specification }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="售价"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.salePrice }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="库存"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.stock }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="上下架"
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
          <el-button type="primary" size="mini" @click="handleEdit(scope.row)">编辑</el-button>
          <el-button :type="!scope.row.isPutAway ? 'success' : 'danger'" size="mini" @click="handleModifyPutAway(scope.row)">{{ scope.row.isPutAway ? '下架' : '上架' }}
          </el-button>
        </template>
      </el-table-column>
    </el-table>
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
      isLoading: false,
      list: null,
      searchParams: {
        itemId: '',
        skuId: '',
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
        itemId: 1,
        skuId: 2,
        productName: 'test',
        productCode: '123456',
        brand: '牛人',
        list: '宇宙',
        specification: 'red',
        salePrice: 2,
        stock: 20,
        isPutAway: true
      }]
    },
    handlerFilter() {
      // search api
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
