<template>
	
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

</template>
<script>

export default {
	name: "mobileCart",
	props: {
		userCart: Object,
		total: Number,
		cart: Boolean,
		cartHeight: Number
	},
	data(){
		return {

		}
	}
}
</script>
