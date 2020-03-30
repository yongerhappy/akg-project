<template>
  <div class="chuck-table">
    <el-table
      :data="tableData"
      stripe
      height="30rem"
      style="width: 100%; border: none">
      <el-table-column
        prop="name"
        width="150"
        class-name="label-class-item"
        >
        <template slot="header" slot-scope="scope">
          <span class="label-class">交易对</span>
        </template>
      </el-table-column>
      <el-table-column label="Chuck指标——分水岭" class-name="header-border">
          <el-table-column
            prop="long1"
            label="1H空头"
            width="120"
            class-name="item-red">
          </el-table-column>
          <el-table-column
            prop="short1"
            label="1H多头"
            width="120"
            class-name="item-green">
          </el-table-column>
          <el-table-column
            prop="long4"
            label="4H空头"
            width="120"
            class-name="item-red">
          </el-table-column>
          <el-table-column
            prop="short4"
            label="4H多头"
            width="120"
            class-name="item-green">
          </el-table-column>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'ChuckTable',
  data () {
    return {
      tableData: []
    }
  },
  created () {
    this.getData()
    setInterval(() => {
      this.getData()
    }, 5000)
  },
  methods: {
    getData () {
      let dataArr = []
      this.$axios.get(this.HOST + '/get_chunk_signal_data')
        .then(res => {
          Object.keys(res.data).forEach(i => {
            dataArr.push({
              name: i,
              long1: res.data[i]['1H']['LONG'].price,
              short1: res.data[i]['1H']['SHORT'].price,
              long4: res.data[i]['4H']['LONG'].price,
              short4: res.data[i]['4H']['SHORT'].price
            })
          })
          this.tableData = dataArr
        })
        .catch(function (error) { // 请求失败处理
          console.log(error)
        })
    }
  }
}
</script>
