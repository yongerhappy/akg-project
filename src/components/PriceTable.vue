<template>
  <div class="price-table">
    <table style="width:100%">
      <tr>
        <th></th>
        <th class="item-price">价格</th>
        <th class="item-blue">M1</th>
        <th class="item-blue">M5</th>
        <th class="item-blue">M15</th>
        <th class="item-blue">M30</th>
        <th class="item-green">H1</th>
        <th class="item-green">H4</th>
        <th class="item-gray">D1</th>
        <th class="item-gray">W1</th>
      </tr>
      <tr class="row-item" v-for="i in Object.keys(this.priceData)" :key="i">
        <td class="row-title">{{i}}</td>
        <td class="item-price">{{priceData[i].price}}</td>
        <td><div :class="setClass(priceData[i].M1.direction)"></div></td>
        <td><div :class="setClass(priceData[i].M5.direction)"></div></td>
        <td><div :class="setClass(priceData[i].M15.direction)"></div></td>
        <td><div :class="setClass(priceData[i].M30.direction)"></div></td>
        <td><div :class="setClass(priceData[i].H1.direction)"></div></td>
        <td><div :class="setClass(priceData[i].H4.direction)"></div></td>
        <td><div :class="setClass(priceData[i].D1.direction)"></div></td>
        <td><div :class="setClass(priceData[i].W1.direction)"></div></td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'PriceTable',
  props: ['priceData'],
  data () {
    return {
      tableData: Object.keys(this.priceData)
    }
  },
  methods: {
    setClass: function (type) {
      const classType = {
        'LONG': 'round-green',
        'SHORT': 'round-red',
        'NEUTRAL': 'round-gray'
      }
      return classType[type]
    }
  }
}
</script>
