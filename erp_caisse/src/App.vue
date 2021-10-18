<template>
  <div id="app">
    <ArticleComp 
    v-for="art in articles"
    v-bind:key="art.name"
    :spec="art"
    v-on:artClick="artClicked"/>

    <hr>
    <Cart
      :cartList="userCart"
      v-on:dropArticle="dropArticle"/>
  </div>
</template>

<script>
import ArticleComp from './components/ArticleComponent.vue'
import Cart from './components/Cart.vue'

export default {
  name: 'App',
  data() {
    return {
      userCart: [
        {
          article: {name: "snikers",prix: 10},
          count: 10
        }
      ],
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
      for(let i = 0; i < this.userCart.length; i++){
        if (this.userCart[i].article.name == spec.name){
          this.userCart[i].count++
          return true;
        }
      }
      this.userCart.push({article:spec,count: 1})
    },
    dropArticle(idx){
      this.userCart.splice(idx,1)
      this.$forceUpdate()
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
