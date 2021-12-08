<!-- style="background-color: #bbff00;" -->
<template>
  <div id="app">
	<v-app v-resize="selectDisplay">
		<Desktop
			v-if="type == 1"
			:products="products"
			:usersData="usersData"/>
		<Mobile
			v-if="type == 2"
			:products="products"
			:usersData="usersData"/>
		
	</v-app>
  </div>
</template>


<script>

import Mobile from './components/Mobile.vue'
import Desktop from './components/Desktop.vue'
import jsonData from './assets/data.json'

export default {

	name: 'App',
	data() {
		return {
			products: jsonData.products,
			usersData: jsonData.usersData,
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
		}
	}
};
</script>
