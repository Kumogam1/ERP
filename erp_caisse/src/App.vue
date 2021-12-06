<!-- style="background-color: #bbff00;" -->
<template>
  <div id="app">
	<v-app>
		<Desktop
		v-if="false "
			:products="products"
			:usersData="usersData"
		/>

		<v-navigation-drawer
			v-model="drawer"
			absolute
			temporary
		>
			<v-list nav>
				<v-list-item-group >
					<v-list-item>
						<v-list-item-title>Foo</v-list-item-title>
					</v-list-item>
				</v-list-item-group>
			</v-list>
		</v-navigation-drawer>
		<v-app-bar-nav-icon 
			class="pl-5"
			@click.stop="drawer = !drawer"></v-app-bar-nav-icon>
		<v-list class="overflow-y-auto" :max-height=windHeight>
			<v-list-item>

			<v-row class="fill-height pa-5">
				<v-col cols="4" 
					class="pa-0"
					v-for="item in products[0].articles"
					v-bind:key="item.name"
				>
					<v-card 
						outlined
						exact
						height="100%">
						<v-card-title> {{item.name}}</v-card-title>
						<v-card-text class="pl-5 font-light-black"> {{item.prix}}€</v-card-text>
						<v-card-actions>
							<v-btn 
								block
								@click="addArt(item)"> ADD </v-btn>
						</v-card-actions>
					</v-card>
				</v-col>
			</v-row>
			</v-list-item>
		</v-list>
		<v-spacer></v-spacer>
		<v-btn 
			@click.stop="cart = !cart"
			block elevation="0"><v-icon>mdi-cart-outline</v-icon></v-btn>
		<v-sheet elevation="0" align="center"> TOTAL : {{total}}€</v-sheet>
		<v-btn block>Valider</v-btn>


		<v-navigation-drawer
			v-model="cart"
			absolute
			right
			temporary
		>
			<v-list class="overflow-y-auto" :max-height=windHeight>
				<v-list-item-group>
					<v-list-item 
						v-for="item in userCart.cart"
						v-bind:key="item.name">
						{{item.article.name}} 
						<v-spacer></v-spacer>
						{{item.count}}
					</v-list-item>
				</v-list-item-group>
			</v-list>
			<template v-slot:append>
				<v-sheet class="pa-5">
					Total : {{total}}
				</v-sheet>
			</template>
		</v-navigation-drawer>
	</v-app>
  </div>
</template>

<style>
.v-btn {
  width: 120x;
  min-width: 120x;
}

</style>

<script>

import Desktop from './components/Desktop.vue'
import jsonData from './assets/data.json'
export default {

	name: 'App',
	data() {
		return {
			drawer: false,
			cart: false,
			windHeight: window.innerHeight - 135,
			products: jsonData.products,
			usersData: jsonData.usersData,
			userCart: {total:0, cart:[]},
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
		total: function () {
            let sum = 0;
            for(let i =0;i < this.userCart.cart.length; i++){
				if (this.userCart.cart[i].type == "buy")
					sum = sum + this.userCart.cart[i].article.prix * this.userCart.cart[i].count;
            }
            return sum;
        }
	},
	components: {
		Desktop,
	},

	methods: {
		addArt(item){
			console.log(item.name)
			for(let i = 0; i < this.userCart.cart.length; i++){
				if (this.userCart.cart[i].article.name == item.name){
					this.userCart.cart[i].count++
					return true;
				}
			}
			this.userCart.cart.push({type:"buy",article:item,count: 1})
		}
	}
};
</script>
