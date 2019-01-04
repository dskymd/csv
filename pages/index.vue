<template>
  <section class="container">
    <div>
      <h1 class="title">csv</h1>
      <h2 class="subtitle">My polished Nuxt.js project</h2>

      orders : {{ totalPrice }} / {{ orderCount }}

      <div class="prices">
        {{prices}}
      </div>

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
    async asyncData({
      params
    }) {
      const {
        data
      } = await axios.get('http://localhost:3000/orders.csv')
      return {
        csv: data
      }
    },
    computed: {
      estimate() {
        let csv = this.csv
        let rows = csv.split('\n')
        // let datas = rows.map(v => v.split(','))
        // var csv_sample = '"学校","我が子の名前=\"健太,綾香\"","特機","鷲見","誕生日"';
        // var csvvalue = csv_sample.split(",(?=([^\"]*\"[^\"]*\")*[^\"]*$)", -1);
        // let datas = rows.map( v => v.split(',([^"]*"[^"]*")*[^"]*$)', -1))
        let datas = rows.map(v => v.match(/"[^"]*"|[^,]+/g)) // /"(\\["ntr\\]|[^"])*"|[^,]+/g
        datas = datas.slice(1, -1) //.shift()
        return datas
      },
      prices() {

        let csv = this.csv
        let rows = csv.split('\n')
        let tmp

        // tmp = rows.map(v => v.match(/"[^"]*"|[^,]+/g))
        tmp = rows.map(v => v.match(/"(\\["ntr\\]|[^"])*"|[^,]+/g))

        return tmp[1]
      },




      totalPrice() {
        let datas = this.estimate
        let tmp = 0
        // let c = 0
        for (let r in datas) {
          if (datas[r][4]) {
            // tmp += ~~datas[r][4].split('￥')[1]

            // console.log( datas[r][4].split('￥')[1] )
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
