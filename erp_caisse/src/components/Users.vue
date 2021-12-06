<template>
	<div>
		<v-card class="mt-10 mb-5 mr-5">
			<v-card-title> Users </v-card-title>
			<v-text-field
				class="pa-5 pt-0"
				label="Search username"
				v-model="searchInput"
				hide-details="auto"
			></v-text-field>
			
		</v-card>
		<v-expansion-panels popout >
			<v-expansion-panel
				v-for="user in items"
				v-bind:key="user.id"
				class="mr-10 ml-5"
			>	
				<v-expansion-panel-header >
					<template>
						{{user.username}}
					</template>
				</v-expansion-panel-header>
				<v-expansion-panel-content>
					<span class="font-weight-black"> Name : </span>{{user.username}} <br/>
					<span class="font-weight-black"> Id : </span>{{user.id}} <br/>
					<span class="font-weight-black"> Credit : </span>{{user.credit}} <br/>
					<span class="font-weight-black"> {{user.username}}'s transactions historic : </span><br/>
					<v-card-text>
						<v-data-table
							:headers="headers"
							:items="user.historic">
						</v-data-table>
					</v-card-text>
				

				</v-expansion-panel-content>
			</v-expansion-panel>
		</v-expansion-panels>
	</div>
</template>

<script>

export default{
	name:"UsersPage",
	props: {
		usersData: Array
	},
	data(){
		return {
			searchInput: "",

			headers: [
				{
					text: 'N° Transaction',
					align: 'start',
					sortable: false,
					value: 'transactionId',
				},
				{ text: 'Date', value: 'date' },
				{ text: 'Transaction', value: 'transactionContent' },
			]
		}
	},
	methods:{
		
	},
	computed:{
		items: function(){
			return this.usersData.filter(e => e.username.includes(this.searchInput))
		}
	}

};
</script>
