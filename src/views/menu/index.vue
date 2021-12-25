<template>
  <div class="menu">
    <el-card class="box-card">
  <div slot="header" class="clearfix">
    <el-button
      @click="$router.push({ name: 'menu-create' })"
    >添加菜单</el-button>
  </div>
  <!-- 底部菜单列表展示 -->
  <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column
        label="编号"
        type="index"
      >
      </el-table-column>
      <el-table-column
        prop="name"
        label="菜单名称"
      >
      </el-table-column>
      <el-table-column
        prop="level"
        label="菜单级数">
      </el-table-column>
      <el-table-column
        prop="icon"
        label="前端图标">
      </el-table-column>
      <el-table-column
        prop="orderNum"
        label="排序">
      </el-table-column>
      <el-table-column
        label="操作"
        width="180">
        <template slot-scope="scope">
        <el-button
          size="mini"
          @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
        <el-button
          size="mini"
          type="danger"
          @click="handleDelete(scope.$index, scope.row)">删除</el-button>
      </template>
      </el-table-column>
    </el-table>
</el-card>
  </div>
</template>

<script>
import { getAllMenus } from '@/services/menu'
export default {
  name: 'MenuIndex',
  data () {
    return {
      tableData: []
    }
  },
  created () {
    this.loadAllMenus()
  },
  methods: {
    handleEdit () {

    },
    handleDelete () {

    },
    // 获取所有菜单信息
    async loadAllMenus () {
      const { data } = await getAllMenus()
      if (data.code === '000000') {
        this.tableData = data.data
      }
    }
  }
}
</script>

<style lang="scss" scoped></style>
