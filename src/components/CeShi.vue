<template>
  <div>
    <el-button type="primary" @click="addClick">新增</el-button>
    <div class="form-wrap">
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="活动名称">
          <el-input v-model="form.name"></el-input>
        </el-form-item>

        <div class="block_item" v-for="(item, index) in list" :key="index">
          <el-form-item label="押运单位">
            <el-select
              v-model="item.selectedYydwId"
              placeholder="请选择押运单位"
              @change="handleYydwChange(index)"
            >
              <el-option
                v-for="yydwItem in yydwOptions"
                :key="yydwItem.value"
                :label="yydwItem.label"
                :value="yydwItem.value"
                :disabled="isYydwDisabled(yydwItem.value)"
              >
              </el-option>
            </el-select>
          </el-form-item>

          <el-form-item label="押运员">
            <el-select
              multiple
              v-model="item.selectedYYYList"
              placeholder="请选择押运员"
              @change="() => handleYyyChange(index)"
            >
              <el-option
                v-for="item in item.yyyOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value"
                :disabled="isYyyDisabled(item.value)"
              >
              </el-option>
            </el-select>
          </el-form-item>

          <el-button @click="delClick(index)" size="small" type="danger"
            >del</el-button
          >
        </div>

        <el-form-item>
          <el-button type="primary" @click="onSubmit">立即创建</el-button>
          <el-button>取消</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [
        {
          selectedYydwName: "", // 选中的押运单位名称
          selectedYydwId: "", // 选中的押运单位id
          selectedYYYList: [], // 选中的押运员列表
          yyyOptions: [], // 押运员选项列表
        },
      ],
      yydwOptions: [
        {
          value: "押运单位1",
          label: "yydw1",
          yyyOptions: [
            { label: "押运员1-1", value: "yyy1_1" },
            { label: "押运员1-2", value: "yyy1_2" },
            { label: "押运员1-3", value: "yyy1_3" },
          ],
        },
        {
          value: "押运单位2",
          label: "yydw2",
          yyyOptions: [
            { label: "押运员2-1", value: "yyy2_1" },
            { label: "押运员2-2", value: "yyy2_2" },
            { label: "押运员2-3", value: "yyy2_3" },
            { label: "押运员3-3", value: "yyy3_3" },
          ],
        },
        {
          value: "押运单位3",
          label: "yydw3",
          yyyOptions: [
            { label: "押运员3-1", value: "yyy3_1" },
            { label: "押运员3-2", value: "yyy3_2" },
            { label: "押运员3-3", value: "yyy3_3" },
          ],
        },
      ],
      form: {
        name: "",
      },
    };
  },
  methods: {
    onSubmit() {
      console.log(this.list);
      console.log("submit!");
    },
    delClick(index) {
      this.list.splice(index, 1);
    },
    addClick() {
      this.list.push({
        selectedYydwName: "", // 选中的押运单位名称
        selectedYydwId: "", // 选中的押运单位id
        selectedYYYList: [], // 选中的押运员列表
        yyyOptions: [], // 押运员选项列表
      });
    },
    handleYydwChange(index) {
      const selectedYydw = this.yydwOptions.find(
        (yydw) => yydw.value === this.list[index].selectedYydwId
      );
      this.list[index].yyyOptions = selectedYydw ? selectedYydw.yyyOptions : [];
      this.list[index].selectedYydwName = selectedYydw.label || "";
      // this.$set(
      //   this.list[index],
      //   "yyyOptions",
      //   selectedYydw ? selectedYydw.yyyOptions : []
      // );
      // this.$set(this.list[index], "selectedYydwName", selectedYydw.label || "");
    },

    isYydwDisabled(value) {
      return this.list.some((item) => item.selectedYydwId === value);
    },
    isYyyDisabled(value) {
      return this.list.some((item) => item.selectedYYYList.includes(value));
    },
    handleYyyChange(currentIndex) {
      const totalSelectedYYY = this.list.reduce(
        (acc, item) => acc.concat(item.selectedYYYList),
        []
      );
      if (totalSelectedYYY.length > 5) {
        this.$message({
          message: "押运员总共最多只能选择5个",
          type: "warning",
        });

        let count = 0;
        for (let i = 0; i < this.list.length; i++) {
          if (i === currentIndex) continue; // 跳过当前单位
          count += this.list[i].selectedYYYList.length;
        }

        // 计算当前单位可以选择的剩余押运员数量
        const remaining = 5 - count;
        if (this.list[currentIndex].selectedYYYList.length > remaining) {
          this.list[currentIndex].selectedYYYList = this.list[
            currentIndex
          ].selectedYYYList.slice(0, remaining);
        }
      }
    },
  },
};
</script>

<style scoped>
.form-wrap {
  width: 500px;
}
.block_item {
  padding: 10px 0;
  border-bottom: 1px solid red;
}
</style>
