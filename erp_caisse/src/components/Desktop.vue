<template>
	<v-row> 
		<v-col cols ="1">
		<v-navigation-drawer
			permanent
		>
			<v-list>
				<v-list-item-group  v-model="selectedSet">
					<v-list-item 
						v-for="item in products"
						v-bind:key="item.name"
						@click="displayShop = true;selectedPage = null;"
					>
						<v-list-item-icon>
							<v-icon large>{{ item.icon }}</v-icon>
						</v-list-item-icon>
						<v-list-item-content></v-list-item-content>
					</v-list-item>
				</v-list-item-group>
			</v-list><v-divider></v-divider>
			<v-list>
				<v-list-item-group v-model="selectedPage">
					<v-list-item 
						v-for="item in menus"
						v-bind:key="item.name"
						@click="setMenu"
					>
						<v-list-item-icon>
							<v-icon large>{{item.icon}}</v-icon>
						</v-list-item-icon>
						<v-list-item-content></v-list-item-content>
					</v-list-item>
				</v-list-item-group>
			</v-list>
			
		</v-navigation-drawer>
		</v-col>
		<v-col>
		<Shop
			v-if="displayShop" 
			:userCart="userCart"
			:set="set"
			v-on:dropArticle="dropArticle"
			v-on:artClick="artClicked"
			v-on:checkout="checkout"
		/>
		<div v-else>
			<CreditEdit
				v-if="selectedPage == 0"
			/>

			<UserPage 
				v-else-if="selectedPage == 2"
				:usersData="usersData"
				/>
		</div>
		</v-col>
	</v-row>
</template>


<script>
import Shop from './Shop.vue'
import UserPage from "./Users.vue"
import CreditEdit from "./CreditEdit.vue"
export default{
	name:"Desktop",
	props:{
		products: Array,
		usersData: Array,
	},
	data() {
		return{
			displayShop: true,
			paid: 0,
			userCart: {total:0, cart:[]},
			
			selectedSet: 0,
			selectedPage: null,
			menus: [
				{name:"addCredit",icon: "mdi-cash-plus"},
				{name:"historic",icon: "mdi-book-arrow-left-outline"},
				{name:"users",icon: "mdi-account-circle-outline"},
				{name:"addEvent",icon: "mdi-plus-circle-outline"},
			]
		}
	},
	components: {
		Shop,
		UserPage,
		CreditEdit
	},
	computed: {
		set: function(){ 
			if (this.selectedSet != null){
				return this.products[this.selectedSet];
			}
			else 
				return this.prevSet
		}
	},
	methods: {
		setMenu(){
			this.displayShop = false;
			this.selectedSet = null;
		},
		artClicked(spec){
			
			for(let i = 0; i < this.userCart.cart.length; i++){
				if (this.userCart.cart[i].article.name == spec.name){
					this.userCart.cart[i].count++
					return true;
				}
			}
			this.userCart.cart.push({type:"buy",article:spec,count: 1})
		},
		dropArticle(idx){
			this.userCart.cart.splice(idx,1)
			this.$forceUpdate()
		},
		checkout(){
			this.paid = this.userCart.total
		}
	}
}
</script>
