<template>
	<div v-resize="onResize">
	<v-navigation-drawer
			v-model="drawer"
			absolute
			permanent
		>
			<v-list nav class="overflow-y-auto" :max-height=computedHeight>
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
							<v-icon >{{item.icon}}</v-icon>
						</v-list-item-icon>
						<v-list-item-title>{{item.name}}</v-list-item-title>
					</v-list-item>

				</v-list-item-group>
			</v-list>
		</v-navigation-drawer>

		<v-row class="ml-10 mr-5">
			<v-col cols="2"></v-col>
			<v-col cols="8">
				<div v-if="displaySet" >
					<v-list class="overflow-y-auto" :max-height=computedHeight >
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
				</div>
				<!-- *************************** USERS LIST ******************* -->
				<div v-else-if="selectedSet == products.length + 0" class="ma-5 mr-10">
					<v-card>
						<v-card-title> Users </v-card-title>
						<v-text-field
							class="pa-5 pt-0"
							label="Search username"
							v-model="searchInput"
							hide-details="auto"
						></v-text-field>
					</v-card>
					<v-list class="overflow-y-auto" :max-height=computedHeight >
						<v-list-item>
							<v-expansion-panels >
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
						</v-list-item>
					</v-list>
				</div>

		<!-- *********************** ADD MONEY ****************************** -->
				<div v-else-if="selectedSet == products.length + 1"
					class="ma-10 ml-5"
				>
					<v-card>
						<v-card-title>Add credit</v-card-title>
						<v-card-text>
							<v-text-field
								v-model="userId"
								label="User ID"
							></v-text-field>
							<v-text-field
								v-model="amount"
								label="Amount"
							></v-text-field>
							<v-text-field
								v-model="pw"
								label="Administrator password"
								:type="show ? 'text' : 'password'"
								:append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
								@click:append="show = !show"
							></v-text-field>
							<v-btn @click="confirmTest()" block text color="primary"> Valid </v-btn>
						</v-card-text>
					</v-card>
				</div>
			</v-col>
		</v-row>


		<v-navigation-drawer
			absolute
			permanent
			right
		>
			<v-card height="50px" elevation="0" flat><v-card-title></v-card-title></v-card>

			<v-list class="overflow-y-auto" :max-height=computedHeight>
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
				<v-btn block @click="clearCart" color="red" text> CLEAR </v-btn>
				<v-sheet class="pa-5">
					Total : {{total}}
				</v-sheet>
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
							@click="pay()"
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
				<v-card height="50px" elevation="0" flat><v-card-title></v-card-title></v-card>
			</template>
		</v-navigation-drawer>
		</div>
</template>


<script>
import axios from 'axios'
export default{
	name:"Desktop",
	props:{
		products: Array,
		usersData: Array,
		config: Object,
	},
	data() {
		return {
			show: false,
			drawer: false,
			cart: false,
			dialog: false,
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
			computedHeight: window.innerHeight - 100,
			userId:"",
			amount:"",
			pw:"",
			token:"ViVeLeWeEkEnD!"
		}
	},
	computed: {
		// Retourne la hauteur de l'écran
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
		// Retourne le prix total du panier
		total: function () {
            let sum = 0;
            for(let i =0;i < this.userCart.cart.length; i++){
				if (this.userCart.cart[i].type == "buy")
					sum = sum + this.userCart.cart[i].article.prix * this.userCart.cart[i].count;
            }
            return sum;
        },
		// Retourne le set à afficher dans la page magasin
		set: function(){
			if (this.displaySet)
				return this.products[this.selectedSet]
			else
				return null
		},
		// Retourne la liste des utilisteurs
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
	},

	methods: {
		pay(){
			console.log();
			var checkoutId;
			var request = {
				method: "POST",
				mode: "cors",
				headers: {
					"Content-Type": "application/json"
				},
				body: JSON.stringify({
					ref: Math.random(),
					amount: this.total,
					desc: "test",
				})
			}

			fetch(this.config.ip + "/checkout", request)
				.then(response => response.text())
				.then(data => {
					checkoutId = data;
					console.log("Checkout ID : " + checkoutId);
					this.dialog = false;
					window.location.href = this.config.ip + "/?checkout_id=" + checkoutId + "&user_id=" + this.searchUser ;
				})
				.catch(error => { console.error(error) })
		},
		// Augmente le compteur d'un objet déjà dans le panier 
		addSome(obj){
            obj.count++;
        },
        // Enléve 1 au compteur d'un objet dans le panier, si le compteur est à 0, on supprime l'objet
        removeSome(obj){
            obj.count--;
            if(obj.count <= 0)
                this.dropArticle(obj);
        },
		// Abandonne un objet du panier
		dropArticle(obj){
            const idx = this.userCart.cart.findIndex(item => item.article.name === obj.article.name )
            this.userCart.cart.splice(idx,1)
			this.$forceUpdate()
        },
		// Ajoute un nouvel objet dans le panier
		addArt(item){
			console.log(item.name)
			// Si l'objet existe déjà, augmenter le compteur
			for(let i = 0; i < this.userCart.cart.length; i++){
				if (this.userCart.cart[i].article.name == item.name){
					this.userCart.cart[i].count++
					return true;
				}
			}
			this.userCart.cart.push({type:"buy",article:item,count: 1})
		},
		// Recalculer la taille
		onResize(){
			this.computedHeight = window.innerHeight - 100
		},
		// Vider le panier
		clearCart(){
			this.userCart.cart = []
		},
		confirmTest(){
			alert('http://erp-student-2021.000webhostapp.com/api.php?token=' + this.token + '&a=add-money&id=' + this.userId + '&w=' + this.amount)
			const response = axios.get('http://erp-student-2021.000webhostapp.com/api.php?token=' + this.token + '&a=add-money&id=' + this.userId + '&w=' + this.amount)
			response.then(function(res) {
				alert(JSON.stringify(res))
			});
		},
		// pay(){
		// 	var date = new Date().toISOString().substr(0, 10).split('-')
		// 	var produit = "Objet"
		// 	const response = axios.get('http://erp-student-2021.000webhostapp.com/api.php?token=' + this.token + '&a=add-transaction&id=' + this.searchUser + '&w=%7B%22date%22:%22' + date[2]+'%2F'+date[1]+'%2F'+date[0] + '%22,%22montant%22:-' + this.total + ',%22article%22:%22' + produit + '%22%7D')
		// 	response.then(function(res) {
		// 		alert(JSON.stringify(res))
		// 	});
		// }
	}
}
</script>
