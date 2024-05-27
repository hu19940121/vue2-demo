<template>
  <div>
    <el-table
      row-key="id"
      ref="multipleTable"
      :data="tableData"
      tooltip-effect="dark"
      style="width: 100%"
      @selection-change="handleSelectionChange"
    >
      <el-table-column type="selection" width="55" :selectable="handleSelectable"> </el-table-column>
      <el-table-column label="日期" width="120">
        <template slot-scope="scope">{{ scope.row.date }}</template>
      </el-table-column>
      <el-table-column prop="name" label="姓名" width="120"> </el-table-column>
      <el-table-column prop="address" label="地址" show-overflow-tooltip>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [
        {
          date: "2016-05-03",
          name: "王小虎1",
          id:1,
          idCardNumber:110,
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-02",
          name: "王小虎2",
          id:2,
          idCardNumber:111,
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-04",
          name: "王小虎3",
          id:3,
          idCardNumber:112,
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-01",
          name: "王小虎4",
          id:4,
          idCardNumber:113,
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-08",
          name: "王小虎5",
          id:5,
          idCardNumber:114,
          address: "上海市普陀区金沙江路 1518 弄",
        },
        {
          date: "2016-05-08",
          name: "王小虎5",
          id:6,
          idCardNumber:114,
          address: "上海市普陀区金沙江路 1518 弄",
        },
      ],
      multipleSelection: [],
    };
  },
  methods: {
    handleSelectionChange(val) {
      this.multipleSelection = val;
      console.log(this.multipleSelection);
    },
    handleSelectable(row) {
      // 检查当前行的 idCardNumber 是否在已选行的 idCardNumber 列表中
      const selectedCardNumbers = this.multipleSelection.map(item => item.idCardNumber);
      return !selectedCardNumbers.includes(row.idCardNumber) || this.multipleSelection.some(item => item.id === row.id);
    }
  }
};
</script>

<style scoped></style>
