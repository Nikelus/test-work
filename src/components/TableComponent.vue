<template>
    <div class='card__content'>
      <div id='league-table' class='table-responsive' >
        <table class='table table--lg team-roster-table table-hover'>
          <thead class="fixedHeader">
            <th class='team-roster-table__name'>Название</th>
            <th class='team-roster-table__name'>Стоимость</th>
            <th class='team-roster-table__name show'>Рыночная капитализация</th>
            <th class='team-roster-table__name show'>Суточный объем</th>
          </thead>
		
          <tbody class="scrollContent">
            <!-- eslint-disable vue/no-use-v-if-with-v-for,vue/no-confusing-v-for-v-if -->
            <tr v-for="(items,index) in even(items)" v-if="index < 15" :key="index" class='scroll-table'>
              <td class='team-roster-table__name'>{{ items.name }}</td>
              <td class='team-roster-table__name'>{{ items.priceUsd }}</td>
              <td class='team-roster-table__name show'>{{ items.marketCapUsd }}</td>
              <td class='team-roster-table__name show'>{{ items.volumeUsd24Hr }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
</template>

<script>
export default {
  name: 'TableComponent',
  props: {
    msg: String
  },
  data() {
    return {
      leagues: [],
      apilink: 'https://api.coincap.io/v2/assets',
      items: []
    }
  },
  mounted: function() {
    this.getLeagues()
  },
  methods: {
    getLeagues: function() {
      let self = this
      fetch(this.apilink)
      .then(function(response) {
        return response.json()
      })
      .then(function(data) {
        self.prepareItems(data.data)
      })
      .catch(console.error('ERROR'))
    },
    prepareItems(itemList) {
      this.items = []
      itemList.forEach((item) => {
        this.items.push({
          name: item.name,
          priceUsd: item.priceUsd,
          marketCapUsd: item.marketCapUsd,
          volumeUsd24Hr: item.volumeUsd24Hr
        })
      })
      return this.items
    },
    even: function(arr) {
      return arr.slice().sort(function(a, b) {
        return b.volumeUsd24Hr - a.volumeUsd24Hr
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
*{
	margin: 0;
	padding: 0;
}

body{
	overflow: hidden;
}

table {
  width: 100vw;
}


thead {
  display: table;
  width: 100vw;
}

tbody {
  display: block;
  border-top: none; 
  max-height: calc(100vh - 50px);
  width: calc(100vw - 17px);
  overflow-y: hidden;
}

tbody:hover{
  width: 100vw;
  overflow-y: scroll;
}

th, td {
  width: 25%;
}

tr {
  display: table;
  width: 100%;
}

td {
  border-bottom: none;
  border-left: none;
}

.table>tbody>tr>td{ 
    padding: 20px 0 20px 10px;
    border-top: none;
}

@media all and (max-width: 992px) {  
 	.show {display: none;}
}

@media all and (max-device-width: 992px) {  
  .table{
    font-size: 21pt;
  }
 	.show {display: none;}
}
</style>
