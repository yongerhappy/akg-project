<template>
  <div class="home">
    <div class="header">
      <img class="logo" src="@/assets/logo.png">
      <el-button class="login" @click="goLogin()">注册/登陆</el-button>
    </div>
    <div class="main-content">
      <div class="main-left">
        <ChunkTable></ChunkTable>
        <div class="carousel">
          <el-carousel :interval=10000>
            <el-carousel-item>
              <img src="@/assets/carousel.jpeg">
            </el-carousel-item>
            <el-carousel-item>
              <img src="@/assets/carousel2.jpeg">
            </el-carousel-item>
          </el-carousel>
        </div>
      </div>
      <div class="main-right">
        <PriceTable :priceData="priceData"></PriceTable>
      </div>
    </div>
    <div class="footer">
      <el-button type="info" icon="el-icon-message" circle @click="openMsg()"></el-button>
    </div>
    <div class="introduction"
      @click="dialogVisible = true">
      <el-tooltip class="item" effect="dark" content="指标说明" placement="left">
        <el-button
          type="success"
          icon="el-icon-document"
          circle
        >
        </el-button>
      </el-tooltip>
    </div>
    <el-dialog
      :visible.sync="dialogVisible"
      width="1200px"
      class="tips-dialog"
      :show-close="true"
      >
      <i class="el-dialog__close el-icon el-icon-close" @click="dialogVisible=false"></i>
      <img src="@/assets/tips.jpeg" class="tips-image">
    </el-dialog>
  </div>
</template>

<script>
import ChunkTable from '@/components/ChunkTable'
import PriceTable from '@/components/PriceTable'
export default {
  data () {
    return {
      priceData: {},
      dialogVisible: false
    }
  },
  components: {
    ChunkTable,
    PriceTable
  },
  created () {
    this.getData()
    setInterval(() => {
      this.getData()
    }, 5000)
  },
  methods: {
    getData () {
      this.$axios.get(this.HOST + '/get_signal_data')
        .then(res => {
          this.priceData = res.data
        })
        .catch(function (error) { // 请求失败处理
          console.log(error)
        })
    },
    openMsg () {
      this.$alert('邮箱：info@akgvc.com', '联系我们', {
        confirmButtonText: '确定'
      })
    },
    goLogin () {
      this.$router.replace('/login')
    }
  }
}
</script>
