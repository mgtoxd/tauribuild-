<script setup>
import NewRule from "./components/newRule.vue";
</script>
<template>

  <new-rule v-if="visibleAdd" :changeDraw="changeDraw"></new-rule>
  <div v-if="!visibleAdd">
    <p>规则列表</p>
    <el-button color="#535353" type="primary" @click="changeDraw">添加</el-button>
    <el-table
        :data="tableData"
        style="width: 100%"
        :row-class-name="tableRowClassName"
    >
      <el-table-column prop="name" label="规则名称" width="180"></el-table-column>
      <el-table-column prop="cmd" label="规则命令" width="180"></el-table-column>
    </el-table>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<script>

export default {
  methods: {
    tableRowClassName({ row, rowIndex }) {
      if (rowIndex === 1) {
        return 'warning-row'
      } else if (rowIndex === 3) {
        return 'success-row'
      }
      return ''
    },
    changeDraw(){
      this.visibleAdd = !this.visibleAdd
    }
  },
  data() {
    return {
      visibleAdd:false,
      tableData: [
        {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
        },
        {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
        },
        {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
        },
        {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄',
        },
      ],
    }
  },
}
</script>
