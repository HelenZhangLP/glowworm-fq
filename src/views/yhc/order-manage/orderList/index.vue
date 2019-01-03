<template>
  <div class="app-container">
    <div class="filter-container">
      <el-input
        v-model="orderSearch.orderId"
        placeholder="订单 ID"
        class="filter-item"
        style="width: 200px"
        @keyup.enter.native="handleFilter"/>
      <el-input
        v-model="orderSearch.machineId"
        placeholder="设备 ID"
        class="filter-item"
        style="width: 200px"
        @keyup.enter.native="handleFilter"/>
      <el-select
        v-model="type"
        placeholder="类型"
        class="filter-item"
        style="width: 100px"
        @change="handleFilter">
        <el-option
          v-for="item in type"
          :key="item"
          :label="item"
          :value="item"/>
      </el-select>
      <el-button
        v-waves
        class="filter-item"
        type="primary"
        icon="el-icon-search"
        @click="handleFilter">搜索</el-button>
    </div>
    <el-table
      v-loading="listLoading"
      :data="list"
      border
      fit
      highlight-current-row
      style="width: 100%">
      <el-table-column
        label="订单 id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="所属区域"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="所属群组"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="设备 id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="状态"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="退款状态"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="用户 id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="用户昵称"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="创建时间"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.dateTime }}</span>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'OrderList',
  data() {
    return {
      orderSearch: {
        orderId: '',
        machineId: '',
        type: [1, 2, 3]
      },
      listLoading: false,
      list: [{
        id: 1,
        dateTime: ''
      }]
    }
  },
  methods: {
    handleFilter() {
      // 搜索
    }
  }
}
</script>

<style scoped>

</style>
