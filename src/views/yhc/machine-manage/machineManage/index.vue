<template>
  <div class="app-container">
    <div class="filter-container">
      <ElInput
        v-model="searchParams.deviceId"
        placeholder="设备 id"
        class="filter-item"
        style="width: 200px"
        @keyup.enter.native="handlerFilter"
      />
      <ElInput
        v-model="searchParams.deviceName"
        placeholder="设备名称"
        class="filter-item"
        style="width: 200px"
        @keyup.enter.native="handlerFilter"
      />
      <ElSelect
        v-model="searchParams.deviceStatus"
        width="100px"
        placeholder="设备状态"
        class="filter-item"
        @change="handlerFilter"
      >
        <ElOption
          v-for="item in deviceOptions"
          :key="item.key"
          :label="item.label"
          :value="item.key"
        />
      </ElSelect>
      <ElSelect
        v-model="searchParams.businessArea"
        style="100px"
        placeholder="商圈"
        class="filter-item"
        @change="handlerFilter"
      >
        <ElOption
          v-for="item in deviceOptions"
          :key="item.key"
          :label="item.label"
          :value="item.key"
        />
      </ElSelect>
      <ElButton
        v-waves
        class="filter-item"
        type="primary"
        icon="el-icon-search"
        @click="handlerFilter"
      >
        搜索
      </ElButton>
      <ElButton
        v-waves
        class="filter-item"
        type="primary"
        icon="el-icon-edit"
        @click="handlerAddItem"
      >
        新增
      </ElButton>
    </div>
    <ElTable
      v-loading="listLoading"
      :data="list"
      border
      highlight-current-row
      style="width: 100%;"
    >
      <ElTableColumn
        :label="tableCellLabel.deviceId"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.deviceName"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.deviceCode"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.deviceType"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.alongArea"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.alongGroup"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.deviceStatus"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.deviceAddress"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.temperature"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.concentration"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.longitude"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.latitude"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.operator"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.createTime"
        prop="id"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.createTime | parseTime('{y}-{m}-{d} {h}:{i}') }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        label="操作"
        width="230"
        align="center"
      >
        <template slot-scope="scope">
          <ElButton type="primary" @click="handlerUpdate(scope.row)">
            编辑
          </ElButton>
          <ElButton :type="scope.row.deviceStatus !== 'stop' ? 'danger' : 'success'">
            {{ scope.row.deviceStatus === 'stop' ? '开启' : '关闭' }}
          </ElButton>
        </template>
      </ElTableColumn>
    </ElTable>
    <ElDialog
      :title="dialogStatus && dialogMap[dialogStatus].title"
      :visible="dialogVisible"
    >
      <ElForm
        ref="dataForm"
        :model="temp"
        :rules="dialogStatus && dialogMap[dialogStatus].rule"
      >
        <ElFormItem
          :label="tableCellLabel.deviceName"
          prop="deviceName"
        >
          <ElInput
            v-model="temp.deviceName"
            :disabled="dialogStatus && dialogMap[dialogStatus].title === 'edit' ? true : false"
            class="filter-item"
          />
        </ElFormItem>
        <ElFormItem
          :lable="tableCellLabel.alongArea"
          prop="alongArea"
        >
          <template>
            <AreaSelect v-model="temp.selected" :level="2" :data="pcaa" type="all" />
          </template>
        </ElFormItem>
      </ElForm>
    </ElDialog>
  </div>
</template>

<script>
import waves from '@/directive/waves'
import { pca, pcaa } from 'area-data'

export default {
  name: 'MachineManage',
  directives: { waves },
  data() {
    return {
      pca: pca,
      pcaa: pcaa,
      tableCellLabel: {
        deviceId: '设备ID',
        deviceName: '设备名称',
        deviceCode: '机器设备号',
        deviceType: '设备类型',
        alongArea: '所属区域',
        alongGroup: '所属群组',
        deviceStatus: '状态',
        deviceAddress: '设备地址',
        temperature: '实时温度',
        concentration: '实时浓度',
        longitude: '经度',
        latitude: '纬度',
        operator: '创建人',
        createTime: '创建时间'
      },
      temp: {},
      listLoading: false,
      dialogStatus: '',
      dialogMap: {
        add: {
          title: '新增',
          rule: {}
        },
        edit: {
          title: '编辑',
          rule: {}
        }
      },
      deviceOptions: [],
      list: [],
      isLoading: false,
      searchParams: {
        deviceStatus: '',
        businessArea: '',
        deviceId: '',
        deviceName: ''
      },
      dialogVisible: false
    }
  },
  created() {
    this.fetchList()
  },
  methods: {
    fetchList() {
      this.list = [
        {
          deviceId: '设备ID',
          deviceName: '设备名称',
          deviceCode: '机器设备号',
          deviceType: '设备类型',
          alongArea: '所属区域',
          alongGroup: '所属群组',
          deviceStatus: '状态',
          deviceAddress: '显示给用户地址',
          temperature: '实时温度',
          concentration: '实时浓度',
          longitude: '经度',
          latitude: '纬度',
          operator: '创建人',
          createTime: '创建时间'
        }
      ]
    },
    handlerFilter() {
      // search
    },
    handlerUpdate(item) {
      // 更新 item
      this.dialogVisible = !this.dialogVisible
      this.dialogStatus = 'edit'
    },
    handlerAddItem() {
      // 新增 item
      this.dialogVisible = !this.dialogVisible
      this.dialogStatus = 'add'
    }
  }
}
</script>

<style scoped>

</style>
