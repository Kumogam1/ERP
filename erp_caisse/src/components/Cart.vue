<template>
    <div>
        <div v-for="art in cartList"
        v-bind:key="art.article.name">
            <p> {{art.article.name}} :  {{art.count}} * {{art.article.prix}}€</p>
            <div>
                <button @click="addSome(art)">+</button>
                <button @click="removeSome(art)">-</button>
                <button @click="dropArticle(art)">SUPPRIME FDP</button>
            </div>
        </div>
        <p> Total : {{total}} €</p>
    </div>
</template>


<script>
export default {
    name: "Cart",
    props: {
        cartList: Array
    },
    data(){
        return {

        }
    },
    computed: {
        total: function () {
            let sum = 0;
            for(let i =0;i < this.cartList.length; i++){
                sum = sum + this.cartList[i].article.prix * this.cartList[i].count;
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
            const idx = this.cartList.findIndex(item => item.article.name === obj.article.name )
            this.$emit("dropArticle",idx)
        }
    }
}
</script>

<style scoped>

</style>