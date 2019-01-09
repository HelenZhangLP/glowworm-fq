<!--suppress ALL -->
<template>
  <div class="app-container">
    <div class="filter-container">
      <ElInput
        v-model="searchParams.areaId"
        class="filter-item"
        style="width: 200px"
        placeholder="请输入区域 ID"
        @keyup.enter.native="handlerFilter"
      />
      <ElInput
        v-model="searchParams.areaName"
        class="filter-item"
        style="width: 200px"
        placeholder="请输入区域名称"
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
      <ElButton
        v-waves
        class="filter-item"
        type="primary"
        icon="el-icon-edit"
        @click="dialogAction()"
      >
        添加
      </ElButton>
    </div>
    <ElTable
      v-loading="isLoading"
      :data="list"
      border
    >
      <ElTableColumn
        :label="tableCellLabel.areaId"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.areaId }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.areaName"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.areaName }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.alongCity"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.alongCity }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.alongArea"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.alongArea }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.address"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.address }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.contacts"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.contacts }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.phone"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.phone }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.creator"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.creator }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        :label="tableCellLabel.createTime"
        align="center"
      >
        <template slot-scope="scope">
          <span>{{ scope.row.createTime }}</span>
        </template>
      </ElTableColumn>
      <ElTableColumn
        label="操作"
        align="center"
      >
        <template slot-scope="scope">
          <ElButton
            type="primary"
            size="mini"
            @click="dialogAction(scope.row)"
          >
            编辑
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
        :rules="dialogStatus && dialogMap[dialogStatus].rules"
        label-width="90px"
      >
        <ElFormItem
          :label="tableCellLabel.areaName"
          prop="areaName"
        >
          <ElInput
            v-model="temp.areaName"
            :placeholder="dialogStatus === 'edit' ? temp.areaName : '请输入区域名称'"
            class="filter-item"
          />
        </ElFormItem>
        <AreaSelect v-model="temp.selected" :data="pcaa" :level="2" type="all" class="test" />
        <ElFormItem
          :label="tableCellLabel.address"
          prop="address">
          <ElInput
            v-model="temp.address"
            :placeholder="dialogStatus === 'edit' ? temp.address : '请输入详细地址'"
            class="filter-item"/>
        </ElFormItem>
        <ElFormItem
          :label="tableCellLabel.creator"
          prop="creator">
          <ElInput
            v-model="temp.creator"
            :placeholder="dialogStatus === 'edit' ? temp.address : '请输入联系人'"
            class="filter-item"/>
        </ElFormItem>
        <ElFormItem
          :label="tableCellLabel.phone"
          prop="phone">
          <ElInput
            v-model="temp.phone"
            :placeholder="dialogStatus === 'edit' ? temp.address : '请输入联系电话'"
            class="filter-item"/>
        </ElFormItem>
      </ElForm>
      <div slot="footer" class="dialog-footer">
        <ElButton
          @click="dialogVisible = false">
          取消
        </ElButton>
        <ElButton
          type="primary"
          @click="dialogStatus === 'edit' ? confirmEdit : confirmAdd ">
          提交
        </ElButton>
      </div>
    </ElDialog>
  </div>
</template>

<script>
import waves from '@/directive/waves'
import { AreaSelect } from 'vue-area-linkage'
import { pca, pcaa } from 'area-data'

export default {
  name: 'AreaManage',
  directives: { waves },
  components: { AreaSelect },
  data() {
    return {
      pca: pca,
      pcaa: pcaa,
      tableCellLabel: {
        // 列表 label
        areaId: '区域ID',
        areaName: '区域名称',
        alongCity: '所在市',
        alongArea: '所在区',
        address: '详细地址',
        contacts: '联系人',
        phone: '联系电话',
        creator: '创建人',
        createTime: '创建时间'
      },
      searchParams: {
        // 搜索参数
        areaId: '',
        areaName: ''
      },
      dialogVisible: false,
      dialogStatus: '',
      dialogMap: {
        'add': {
          title: '新增',
          rules: {
            areaName: [{ required: true, message: '区域名称为必填', trigger: 'blur' }]
          }
        },
        'edit': {
          title: '编辑',
          rules: {
            areaName: [{ required: true, message: '区域名称为必填', trigger: 'blur' }]
          }
        }
      },
      isLoading: false,
      list: [],
      temp: {
        // 添加、编辑 data
        areaName: '',
        areaSelected: ''
      }
    }
  },
  created() {
    this.fetchList()
  },
  methods: {
    fetchList() {
      // 获取列表
      this.list = [{
        // 列表 label
        areaId: '区域ID',
        areaName: '区域名称',
        alongCity: '所在市',
        alongArea: '所在区',
        address: '详细地址',
        contact: '联系人',
        phone: '联系电话',
        creator: '创建人',
        createTime: '创建时间'
      }]
    },
    handlerFilter() {
      // 搜索
    },
    dialogAction(item) {
      // 添加 item dialog
      this.dialogVisible = !this.dialogVisible
      this.dialogStatus = item ? 'edit' : 'add'
      if (item) this.temp = item
    },
    confirmAdd() {
      // 添加区域 action temp
    },
    confirmEdit() {
      // 添加区域 action temp
    }
  }
}
</script>
<style scoped lang="sass">
  .area-select-wrap
    margin-bottom: 22px
    &:before
      content: '所属区域'
      margin-left: 23px
      font-weight: bold
      line-height: 32px
</style>
