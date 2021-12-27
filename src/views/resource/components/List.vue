<template>
    <div class="resource-list">
        <el-card class="box-card">
            <div slot="header" class="clearfix">
                <!-- 使用 Form 组件: 行内表单 -->
                <el-form :inline="true" :model="form" class="demo-form-inline">
                    <el-form-item label="资源名称">
                        <el-input v-model="form.name" placeholder="资源名称"></el-input>
                    </el-form-item>
                    <el-form-item label="资源路径">
                        <el-input v-model="form.url" placeholder="资源路径"></el-input>
                    </el-form-item>
                    <el-form-item label="资源分类">
                        <el-select v-model="form.categoryId" placeholder="资源分类">
                            <el-option label="区域一" value="shanghai"></el-option>
                            <el-option label="区域二" value="beijing"></el-option>
                        </el-select>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary">查询</el-button>
                    </el-form-item>
                </el-form>
            </div>
                <!-- 使用 Table 组件 -->
                <el-table
                  :data="resources"
                  style="width: 100%">
                <el-table-column
                  type="index"
                  label="编号"
                  width="100">
                </el-table-column>
                <el-table-column
                  prop="name"
                  label="资源名称">
                </el-table-column>
                <el-table-column
                  prop="url"
                  label="资源路径">
                </el-table-column>
                <el-table-column
                  prop="description"
                  label="描述">
                </el-table-column>
                <!-- 设置过滤器需要使用作用域插槽获取数据 -->
                <el-table-column
                  label="添加时间">
                  <template slot-scope="scope">
                    <span>{{ scope.row.createdTime | dateFormat }}</span>
                  </template>
                </el-table-column>
                <el-table-column
                  label="操作">
                  <template slot-scope="scope">
                      <el-button
                        size="mini"
                        @click="handleEdit(scope.row)"
                      >编辑</el-button>
                      <el-button
                        size="mini"
                        @click="handleDelete(scope.row)"
                        type="danger"
                      >删除</el-button>
                  </template>
                </el-table-column>
                </el-table>
                <!-- 分页组件结构 -->
                <el-pagination
                  @size-change="handleSizeChange"
                  @current-change="handleCurrentChange"
                  :current-page="form.current"
                  :page-sizes="[10, 15, 20]"
                  :page-size="form.size"
                  layout="total, sizes, prev, pager, next, jumper"
                  :total="totalCount">
               </el-pagination>
        </el-card>
    </div>
</template>

<script>
import { getResourcePages } from '@/services/resource'
export default {
  name: 'ResourceList',
  data () {
    return {
      // 用于存储资源列表数据
      resources: [],
      form: {
        // 当前显示的页号
        current: 1,
        // 每页显示的数据条数
        size: 10,
        // 资源名称
        name: '',
        // 资源路径
        url: '',
        // 资源分类 ID
        categoryId: ''
      },
      // 数据总数
      totalCount: 0
    }
  },
  created () {
    this.loadResources()
  },
  methods: {
    // 每条显示条数变化时触发
    handleSizeChange (val) {
      this.form.size = val
      // 由于修改了每页显示的条数，应当将页数还原为默认值 1
      this.form.current = 1
      this.loadResources()
    },
    // 页号改变触发
    handleCurrentChange (val) {
      this.form.current = val
      this.loadResources()
    },
    async loadResources () {
      const { data } = await getResourcePages({
        current: this.form.current,
        size: this.form.size
      })
      if (data.code === '000000') {
        this.resources = data.data.records
        this.totalCount = data.data.total
      }
    },
    handleEdit () {
    },
    handleDelete () {
    }
  },
  filters: {
    // 日期过滤器
    dateFormat (date) {
      date = new Date(date)
      return `
        ${date.getFullYear()}-${date.getMonth() + 1}-${date.getDate()}
        ${date.getHours()}:${date.getMinutes()}:${date.getSeconds()}
      `
    }
  }
}
</script>

<style lang="scss" scoped></style>
