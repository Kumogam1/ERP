<!-- style="background-color: #bbff00;" -->
<template>
  <div id="app">
    <v-app>
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
							@click="displayShop = true;selectedPage = null"
						>
							<v-list-item-icon>
								<v-icon x-large>{{ item.icon }}</v-icon>
							</v-list-item-icon>
							<v-list-item-content>
								{{item.name}}
							</v-list-item-content>
						</v-list-item>
					</v-list-item-group>
				</v-list>

				<v-divider></v-divider>
				<v-list>
					<v-list-item-group v-model="selectedPage">
						<v-list-item @click="displayShop = false;selectedSet = null">
							<v-list-item-icon>
								<v-icon x-large>mdi-book-arrow-left-outline</v-icon>
							</v-list-item-icon>
							<v-list-item-content></v-list-item-content>
						</v-list-item>
						<v-list-item @click="displayShop = false;selectedSet = null">
							<v-list-item-icon>
								<v-icon x-large>mdi-account-circle-outline</v-icon>
							</v-list-item-icon>
							<v-list-item-content></v-list-item-content>
						</v-list-item>
						<v-list-item @click="displayShop = false;selectedSet = null">
							<v-list-item-icon>
								<v-icon x-large>mdi-plus-circle-outline</v-icon>
							</v-list-item-icon>
							<v-list-item-content></v-list-item-content>
						</v-list-item>
					</v-list-item-group>
				</v-list>
				
			</v-navigation-drawer>
		</v-col>
		<v-col>
			<Main
				v-if="displayShop" 
				:userCart="userCart"
				:set="set"
				v-on:dropArticle="dropArticle"
				v-on:artClick="artClicked"
				v-on:checkout="checkout"
			/>
		</v-col>
		</v-row>
	</v-app>
  </div>
</template>

<script>
import Main from './components/Main.vue'

export default {

	name: 'App',
	data() {
		return {
			displayShop: true,
			paid: 0,
			userCart: {total:0, cart:[]},
			selectedSet: 0,
			selectedPage: null,
			products: [
				{
					name: "set1",
					icon : 'mdi-home',
					articles: [
						{name: "s1_art1",prix: 10},
						{name: "s1_art2",prix: 10},
						{name: "s1_art3",prix: 10},
						{name: "s1_art4",prix: 10},
						{name: "s1_art5",prix: 10},
						{name: "s1_art6",prix: 10},
						{name: "s1_art7",prix: 10},
						{name: "s1_art8",prix: 10},
						{name: "s1_art9",prix: 10},
						{name: "s1_art10",prix: 10},
						{name: "s1_art11",prix: 10},
						{name: "s1_art12",prix: 10},
						{name: "s1_art13",prix: 10},
						{name: "s1_art14",prix: 10},
						{name: "s1_art15",prix: 10},
						{name: "s1_art16",prix: 10},
						{name: "s1_art17",prix: 10},
						{name: "s1_art18",prix: 10},
						{name: "s1_art19",prix: 10},
						{name: "s1_art20",prix: 10},
						{name: "s1_art21",prix: 10},
					]
				},
				{
					name: "set2",
					icon : 'mdi-alpha-e-circle-outline',
					articles: [
						{name: "s2_art1",prix: 10},
						{name: "s2_art2",prix: 10},
						{name: "s2_art3",prix: 10},
						{name: "s2_art4",prix: 10},
						{name: "s2_art5",prix: 10},
						{name: "s2_art6",prix: 10},
						{name: "s2_art7",prix: 10},
						{name: "s2_art8",prix: 10},
					]
				}
			],
		}
	},
	computed: {
		set: function(){
			return this.products[this.selectedSet];
		}
	},
	components: {
		Main
	},

	methods: {
		addPanel(){
			this.selectedSet = null
			this.selectedSet = 0
		},

		artClicked(spec){
			console.log(this.selectedSet)
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
};
</script>
