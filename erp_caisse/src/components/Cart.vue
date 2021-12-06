<template>
    <!-- <div style="background-color: #ABBAEA"> -->
        <v-card height="100%" class="d-flex flex-column"> 
            <v-spacer></v-spacer>
            <v-list class="overflow-y-auto" :max-height=windHeight>
                <v-list-item-group
                    color="primary"
                >
                    <v-list-item
                    v-for="(item, i) in userCart.cart"
                    :key="i"
                    >
                        <v-list-item-content>
                            <v-row v-if='item.type == "buy"'>
                                <v-col cols="6">
                                    <span class="font-weight-black"> 
										{{item.article.name}}
									</span> 
                                    <span class="font-weight-thin"> 
										- {{item.article.prix}}€/u
                                    <br/>
										Quantity : {{item.count}} 
									</span>
                                    <br/>
                                    Price : {{item.count*item.article.prix}} €
                                </v-col>
                                <v-col cols="6">
                                    <v-row>
                                        <v-col cols="3" class="pl-0 pr-0">
                                            <v-btn @click="addSome(item)" block>+</v-btn>
                                        </v-col>
                                        <v-col cols="3" class="pl-0 pr-0">
                                            <v-btn @click="removeSome(item)" block>-</v-btn>
                                        </v-col>
                                        <v-col cols="3" class="pl-0 pr-0">
                                            <v-btn @click="dropArticle(item)" block>Del</v-btn>
                                        </v-col>
                                    </v-row>
                                </v-col>
                            </v-row>
                        </v-list-item-content>
                    </v-list-item>
                </v-list-item-group>
            </v-list>
			<p class="font-weight-black mt-5 ml-5"> 
				Total : {{total}} €
			</p>
            <!-- <v-card-title> <p>Total : </p> </v-card-title> -->
            <v-card-actions> 
                <v-btn @click="validateCart" block> Valider </v-btn> 
            </v-card-actions>
        </v-card>
    <!-- </div> -->
</template>


<script>
export default {
    name: "Cart",
    props: {
        userCart: Object,
		windHeight: Number
    },
    data(){
        return {
            
        }
    },
    computed: {
        total: function () {
            let sum = 0;
            for(let i =0;i < this.userCart.cart.length; i++){
				if (this.userCart.cart[i].type == "buy")
					sum = sum + this.userCart.cart[i].article.prix * this.userCart.cart[i].count;
            }
            return sum;
        }
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
            this.$emit("dropArticle",idx)
        },

        validateCart(){
            this.userCart.total = this.total
            this.$emit("checkout")
        }
    }
}
</script>

<style scoped>

</style>