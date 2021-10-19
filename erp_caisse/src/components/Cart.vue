<template>
    <div>
        <div v-for="art in userCart.cart"
        v-bind:key="art.article.name">
            <p> {{art.article.name}} :  {{art.count}} * {{art.article.prix}}€</p>
            <div>
                <button @click="addSome(art)">+</button>
                <button @click="removeSome(art)">-</button>
                <button @click="dropArticle(art)">SUPPRIME FDP</button>
                <button @click="validateCart"> Acheter</button>
            </div>
        </div>
        <p> Total : {{total}} €</p>
    </div>
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