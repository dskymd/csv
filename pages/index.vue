<template>
  <section class="container">
    <div>
      <h1 class="title">csv</h1>
      <h2 class="subtitle">My polished Nuxt.js project</h2>

      orders : {{ totalPrice }} / {{ orderCount }}

      <div class="info">{{ estimate }}</div>
    </div>
  </section>
</template>

<script>
  import axios from 'axios'

  export default {
    // components: {
    //   axios
    // },
    // ライフサイクル
    mounted() {
      console.log('mounted!!')
    },
    data() {
      return {
        orders: '1'
      }
    },
    async asyncData({ params }) {
      const { data } = await axios.get('http://localhost:3000/orders.csv')
      return { csv: data }
    },
    computed: {
      estimate() {
        let csv = this.csv
        let rows = csv.split('\n')
        let datas = rows.map(v => v.split(','))
        // let datas = rows.map( v => v.split(',([^"]*"[^"]*")*[^"]*$)', -1))
        datas = datas.slice(1, -1) //.shift()
        return datas
      },

      totalPrice() {
        let datas = this.estimate
        let tmp = 0
        let c = 0
        for (let r in datas) {
          c += 1
          if (~~datas[r][4] > 0) {
            tmp += ~~datas[r][4]
          }
        }
        return tmp
      },
      orderCount() {
        let datas = this.estimate
        let tmp = 0
        // for( let r in datas)
        // {

        //   tm
        // }
        return datas.length
      }
    }
  }
</script>

<style>
  .info {
    padding: 20px;
    background: #f90;
    margin: 20px;
    font-size: 10px;
  }
</style>
