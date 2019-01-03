<template>
  <div class="app-container">
    <div class="filter-container">
      <el-input
        v-model="searchParams.deviceId"
        placeholder="设备 id"
        class="filter-item"
        style="width: 200px"
        @keyup.enter.native="handlerFilter"/>
      <el-input
        v-model="searchParams.deviceName"
        placeholder="设备名称"
        class="filter-item"
        style="width: 200px"
        @keyup.enter.native="handlerFilter"/>
      <el-select
        v-model="searchParams.deviceStatus"
        style="100px"
        placeholder="设备状态"
        class="filter-item"
        @change="handlerFilter">
        <el-option
          v-for="item in deviceOptions"
          :key="item.key"
          :label="item.label"
          :value="item.key"/>
      </el-select>
      <el-select
        v-model="searchParams.businessArea"
        style="100px"
        placeholder="商圈"
        class="filter-item"
        @change="handlerFilter">
        <el-option
          v-for="item in deviceOptions"
          :key="item.key"
          :label="item.label"
          :value="item.key"/>
      </el-select>
      <el-button
        v-waves
        class="filter-item"
        type="primary"
        icon="el-icon-search"
        @click="handlerFilter">搜索</el-button>
    </div>
    <el-table
      v-loading="listLoading"
      :data="list"
      border
      highlight-current-row
      style="width: 100%;">
      <el-table-column
        label="设备 id"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="设备名称"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="机器设备号"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="设备类型"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="所属区域"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="所属群组"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="状态"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="显示给用户地址"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="实时温度"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="实时浓度"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="设备经度"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="设备纬度"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="创建人"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="创建时间"
        prop="id"
        align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import waves from '@/directive/waves'
export default {
  name: 'MachineManage',
  directives: { waves },
  data() {
    return {
      deviceOptions: [],
      searchParams: {
        deviceStatus: '',
        businessArea: '',
        deviceId: '',
        deviceName: ''
      }
    }
  },
  methods: {
    handlerFilter() {
      // search
    }
  }
}
</script>

<style scoped>

</style>
