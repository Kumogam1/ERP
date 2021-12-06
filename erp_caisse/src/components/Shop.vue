<template>
	<v-row class="fill-height" v-resize="onResize">
		<v-col cols="8" class="pl-5 pa-10">
			<p class="font-weight-black">  {{set.name}} </p>
			<v-list class="overflow-y-auto" :max-height=windHeight > 
				<v-list-item>
					<v-row >
							<ArticleComp  
							v-for="art in set.articles"
							v-bind:key="art.name"
							:spec="art"
							v-on:artClick="artClicked"/>
					</v-row>
				</v-list-item>
			</v-list>
		</v-col>

		<v-col cols="4" >
			<Cart
				:userCart="userCart"
				:windHeight="windHeight"
				v-on:dropArticle="dropArticle"
				v-on:checkout="checkout"/>
		</v-col>
	</v-row>
</template>


<script>
import ArticleComp from './ArticleComponent.vue'
import Cart from './Cart.vue'

export default {
	name: "Shop",
    props: {
        userCart: Object,
		set: Object
    },
	data(){
		return {
			windHeight: window.innerHeight - 200
		}
	},
	components: {
		ArticleComp,
		Cart
	},
	methods: {
		artClicked(spec){
			this.$emit("artClick",spec)
		},
		dropArticle(idx){
			this.$emit("dropArticle",idx)
		},
		checkout(){
			this.$emit("checkout")
		},
		onResize(){
			this.windHeight = window.innerHeight - 100
		}
	}
};
</script>
