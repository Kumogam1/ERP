<!-- style="background-color: #bbff00;" -->
<template>
  <div id="app">
	<v-app v-resize="onResize">
		<Desktop
		v-if="false "
			:products="products"
			:usersData="usersData"
		/>
<!-- ***************************** MENU ********************** -->
		<v-navigation-drawer
			v-model="drawer"
			absolute
			temporary
		>
			<v-list nav class="overflow-y-auto" :max-height=cartHeight*2>
				<v-list-item-group v-model="selectedSet">
					<v-list-item
						v-for="item in products"
						v-bind:key="item.name"
						@click="displaySet = true">
						<v-list-item-icon><v-icon>{{item.icon}}</v-icon></v-list-item-icon>
						<v-list-item-title>{{item.name}}</v-list-item-title>
					</v-list-item>
					<v-divider></v-divider>
					<v-list-item 
						v-for="item in menus"
						v-bind:key="item.name"
						@click="displaySet = false"
					>
						<v-list-item-icon>
							<v-icon large>{{item.icon}}</v-icon>
						</v-list-item-icon>
						<v-list-item-title>{{item.name}}</v-list-item-title>
					</v-list-item>

				</v-list-item-group>
			</v-list>
		</v-navigation-drawer>
		<v-app-bar-nav-icon 
			class="pl-5"
			@click.stop="drawer = !drawer"
		></v-app-bar-nav-icon>



<!-- ***************************** SHOP ******************* -->
		<div v-if="displaySet">
			<v-list class="overflow-y-auto" :max-height=windHeight >
				<v-list-item>
				<v-row class="fill-height pa-5">
					<v-col cols="4" 
						class="pa-0"
						v-for="item in set.articles"
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

			<v-dialog
				v-model="dialog"
				width="500"
			>
				<template v-slot:activator="{ on, attrs }">
					<v-btn block 
						text
						color="primary"
						v-bind="attrs"
						v-on="on">Valid</v-btn>
				</template>
				<v-card class="overflow-y-auto">
					<v-card-title>Log user - {{searchUser}}</v-card-title>
					<v-text-field 
						class="pa-10" 
						v-model=searchUser
						label="User ID" ></v-text-field>
					
					<v-btn
						color="green"
						text
						@click="dialog = false"
						block
					>
						Pay
					</v-btn>
					<v-btn
						color="red"
						text
						@click="dialog = false"
						block
					>
						Cancel
					</v-btn>
				</v-card>
			</v-dialog>

		</div>
<!-- *************************** USERS LIST ******************* -->
		<div v-else-if="selectedSet == products.length + 0">  
			<v-card> 
				<v-card-title> Users </v-card-title>
				<v-text-field
					class="pa-5 pt-0"
					label="Search username"
					v-model="searchInput"
					hide-details="auto"
				></v-text-field>
			</v-card>
			<v-expansion-panels>
				<v-expansion-panel
					v-for="item in users"
					v-bind:key="item.id"
				>
					<v-expansion-panel-header>
						{{item.username}}
					</v-expansion-panel-header>
					<v-expansion-panel-content>
						<span class="font-weight-black"> Name : </span>{{item.username}} <br/>
						<span class="font-weight-black"> Id : </span>{{item.id}} <br/>
						<span class="font-weight-black"> Credit : </span>{{item.credit}} <br/>
						<span class="font-weight-black"> {{item.username}}'s transactions historic : </span><br/>
						
						<v-data-table
							:headers="headers"
							:items="item.historic">
						</v-data-table>
					</v-expansion-panel-content>
				</v-expansion-panel>
			</v-expansion-panels>
		</div>

<!-- *********************** ADD MONEY ****************************** -->
		<div v-else-if="selectedSet == products.length + 1">
			<v-card>
				<v-card-title>Add credit</v-card-title>
				<v-card-text>
					<v-text-field 
						label="User ID"
					></v-text-field>
					<v-text-field 
						label="Amount"
					></v-text-field>
					<v-text-field
						label="Administrator password"
						:type="show ? 'text' : 'password'"
						:append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
						@click:append="show = !show"
					></v-text-field>
					<v-btn block text color="primary"> Valid </v-btn>
				</v-card-text>
			</v-card>
		</div>

<!-- ************************* CART ************************* -->

		<v-navigation-drawer
			v-model="cart"
			absolute
			right
			temporary
		>
			<v-list class="overflow-y-auto" :max-height=cartHeight>
				<v-list-item-group>
					<v-list-item 
						v-for="item in userCart.cart"
						v-bind:key="item.name">
						{{item.article.name}} 
						<v-spacer></v-spacer>
						<v-btn fab elevation="0" x-small class="mr-2"
							@click="addSome(item)"
						>
							<v-icon>mdi-plus-thick</v-icon>
						</v-btn>
						{{item.count}}
						<v-btn fab elevation="0" x-small class="ml-2"
							@click="removeSome(item)"
						>
							<v-icon>mdi-minus-thick</v-icon>
						</v-btn>
					</v-list-item>
				</v-list-item-group>
			</v-list>
			<template v-slot:append>
				<v-sheet class="pa-5">
					Total : {{total}}
				</v-sheet>
				<v-btn block @click="clearCart" color="red" text> CLEAR </v-btn>
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
			show: false,
			drawer: false,
			cart: false,
			dialog: false,
			windHeight: window.innerHeight - 150,
			products: jsonData.products,
			usersData: jsonData.usersData,
			userCart: {total:0, cart:[]},
			selectedSet: 0,
			displaySet: true,
			menus: [
				{name:"Users list",icon: "mdi-account-circle-outline"},
				{name:"Add Credit",icon: "mdi-cash-plus"},
				//{name:"Add Event",icon: "mdi-plus-circle-outline"},
			],
			headers: [
				{
					text: 'N° Transaction',
					align: 'start',
					sortable: false,
					value: 'transactionId',
				},
				{ text: 'Date', value: 'date' },
				{ text: 'Transaction', value: 'transactionContent' },
			],
			searchInput:"",
			searchUser:"",
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
        },
		set: function(){
			if (this.displaySet)
				return this.products[this.selectedSet]
			else 
				return null
		},
		cartHeight(){
			return this.windHeight + 30
		},
		users: function(){
			return this.usersData.filter(e => e.username.includes(this.searchInput))
		},
		foundUser: function(){
			let res = this.usersData.filter(e => e.id.includes(this.searchInput))
			if (res.length == 1)
				return "User found."
			else 
				return "Not found"
		}
	},
	components: {
		Desktop
	},

	methods: {
		addSome(obj){
            obj.count++;
        },
        removeSome(obj){
            obj.count--;
            if(obj.count <= 0)
                this.dropArticle(obj);
        },
		dropArticle(obj){
            const idx = this.userCart.cart.findIndex(item => item.article.name === obj.article.name )
            this.userCart.cart.splice(idx,1)
			this.$forceUpdate()
        },
		addArt(item){
			console.log(item.name)
			for(let i = 0; i < this.userCart.cart.length; i++){
				if (this.userCart.cart[i].article.name == item.name){
					this.userCart.cart[i].count++
					return true;
				}
			}
			this.userCart.cart.push({type:"buy",article:item,count: 1})
		},
		onResize(){
			this.windHeight = window.innerHeight - 150
		},
		clearCart(){
			this.userCart.cart = []
		}
		
	}
};
</script>
