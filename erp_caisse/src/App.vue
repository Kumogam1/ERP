<template>
  <div id="app">
    <ArticleComp 
    v-for="art in articles"
    v-bind:key="art.name"
    :spec="art"
    v-on:artClick="artClicked"/>

    <hr>
    <Cart
      :userCart="userCart"
      v-on:dropArticle="dropArticle"
      v-on:checkout="checkout"/>
    
    <span>Prix du panier validé : {{ paid }}</span>
  </div>
</template>

<script>
import ArticleComp from './components/ArticleComponent.vue'
import Cart from './components/Cart.vue'

export default {
  name: 'App',
  data() {
    return {
      paid: 0,
      userCart: {total:0, cart:[]},
      articles: [{
        name: "snikers",
        prix: 10
        },
        {
          name: "julien",
          prix: 40000
        },
        {
          name: "l-word",
          prix: 2
        },
        {
          name: "n-pass",
          prix: 100
        }
      ]
    }
  },
  components: {
    ArticleComp,
    Cart
  },
  methods: {
    artClicked(spec){
      for(let i = 0; i < this.userCart.cart.length; i++){
        if (this.userCart.cart[i].article.name == spec.name){
          this.userCart.cart[i].count++
          return true;
        }
      }
      this.userCart.cart.push({article:spec,count: 1})
    },
    dropArticle(idx){
      this.userCart.cart.splice(idx,1)
      this.$forceUpdate()
    },
    checkout(){
      this.paid = this.userCart.total
    }
  }
  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
