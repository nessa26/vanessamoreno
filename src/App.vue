<template>
  <div class="container">
    <div>
      <div class="min_container">
        <span>
           <h1>NESSA</h1>
          <i>CRYPTO MONEDA</i>
        </span>
        <span>
          <a href="login.vue"><button type="button" class="btn btn-dark">Cerrar sesión</button></a>
        </span>
      </div>
      <input type="text" name="" id="" class="form-control bg-info bg-info.bg-gradient text-dark rounded-0 my-4 " placeholder="Buscar moneda"
        @keyup="searchCoin()"
        v-model="textSearch"
      >
      <table class="table table-hover table-danger">
        <thead>
            <tr class="sub_titles">
              <th v-for="title in titles" :key="title">
              {{title}}
              </th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(coin, index) in filteredCoin" :key="coin.id" class="coin_ico">
            <td>
              {{index}}
            </td>
              <td>
                <img class="icono_img" :src="coin.image" alt="">
              <span>
                {{coin.name}}
              </span>
              <span class="symbol">
                {{coin.symbol}}
              </span>
              </td>
              <td class="text">
                <b>$</b> {{coin.current_price}}
              </td>
              <td class="text">
                {{coin.price_change_percentage_24h}} <b>%</b>
              </td>
              <td class="text">
                <b>$ </b>{{coin.total_volume.toLocaleString()}}
              </td>
            </tr>
        </tbody>
      </table>
    </div>  
  </div>
</template>

<script>
export default {
  name: 'App', 
  data(){
    return{
      coins: [],
      filteredCoin:[],
      titles: [
        '#',
        'MONEDA',
        'PRECIO',
        'CAMBIO DE PRECIO',
        '24h Volume'
      ],
      textSearch:``
    }
  },
  methods:{
    searchCoin(){
      this.filteredCoin = this.coins.filter((coin) => 
      coin.name.toLowerCase().includes(this.textSearch.toLocaleLowerCase()) || 
      coin.symbol.toLowerCase().includes(this.textSearch.toLocaleLowerCase())
      );
      // console.log(this.textSearch)// Pruebas
      // this.coins.filter(coin => coin.name === this.textSearch)
    },
  },
  async mounted() {
    const res= await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false%27')
    const data = await res.json()
    // console.log(data)//PRUEBAS
    this.coins = data
    this.filteredCoin = data
  },
};
</script>