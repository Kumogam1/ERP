<template>
    <v-card>
        <div v-for="art in userCart.cart"
        v-bind:key="art.article.name">
            <v-row>
                <v-col cols="5">
                    {{art.article.name}} : {{art.count}} * {{art.article.prix}}€
                </v-col>
                <v-col cols="7">
                    <v-btn @click="addSome(art)">+</v-btn>
                    <v-btn @click="removeSome(art)">-</v-btn>
                    <v-btn @click="dropArticle(art)">SUPPRIME FDP</v-btn>
                </v-col>
            </v-row>
        </div>
        <p> Total : {{total}} €</p>
        <v-btn @click="validateCart" block> Acheter</v-btn> 
    </v-card>
</template>


<script>
export default {
    name: "Cart",
    props: {
        userCart: Object
    },
    data(){
        return {
            
        }
    },
    computed: {
        total: function () {
            let sum = 0;
            for(let i =0;i < this.userCart.cart.length; i++){
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