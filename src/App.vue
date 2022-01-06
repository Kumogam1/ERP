<!-- style="background-color: #bbff00;" -->
<template>
  <div id="app">
	<v-app v-resize="selectDisplay">
		<!-- Affichage en fonction de la taille de l'écran
			mode pc ou mobile
		-->
		<Desktop
			v-if="type == 1"
			:products="products"
			:usersData="usersData"
			:config="config"/>
		<Mobile
			v-if="type == 2"
			:products="products"
			:usersData="usersData"
			:config="config"/>

	</v-app>
  </div>
</template>


<script>

import Mobile from './components/Mobile.vue'
import Desktop from './components/Desktop.vue'
import jsonData from './assets/data.json'
import configData from './assets/configFile.json'
import axios from 'axios'

export default {

	name: 'App',
	data() {
		return {
			products: jsonData.products,
			usersData: jsonData.usersData,
			config: configData,
			type: 1,
		}
	},
	computed: {
		height () {
			switch (this.$vuetify.breakpoint.name) {
			case 'xs': return 220
			case 'sm': return 400
			case 'md': return 500
			case 'lg': return 600
			case 'xl': return 800
			default : return 800
			}
		},
	},
	components: {
		Mobile,
		Desktop
	},

	methods: {
		selectDisplay(){
			if (window.innerHeight > 500){
				this.type = 1
			}
			else
				this.type = 2
		},
    created() {
      console.log("coucou");

      fetch( 'http://localhost:3000/demande_stock', {mode: "cors", method: "get"} )
        .then((response) => response.json())
        .then( (stock) => {
          console.log(stock);
        })

      axios.get('http://erp-student-2021.000webhostapp.com/api.php?token=' + this.token + '&a=get-infos').then(function(res) {
        console.log(JSON.stringify(res))
      })}

	}

}
</script>
