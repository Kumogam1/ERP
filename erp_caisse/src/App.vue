<template>
  <div id="app">
    <v-app>
      <v-content>
        <v-container fluid  fill-height>
          <v-row>
            <v-col cols="8">
              <v-row>
                <ArticleComp 
                v-for="art in articles"
                v-bind:key="art.name"
                :spec="art"
                v-on:artClick="artClicked"/>
              </v-row>
            </v-col>

            <v-col cols="4">
              <Cart
                :userCart="userCart"
                v-on:dropArticle="dropArticle"
                v-on:checkout="checkout"/>
            </v-col>
          </v-row>
          <span>Prix du panier validé : {{ paid }}</span>
        </v-container>
      </v-content>
    </v-app>
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
};
</script>
