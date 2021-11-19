<template>
    <v-col  cols="3">
        <v-card> 
            <v-card-title>
                {{ spec.name }}
            </v-card-title>
            <v-card-text>
                <p> {{ spec.prix }}€  </p>
                <v-btn @click="addArticle(spec)" block> Ajouter </v-btn>
            </v-card-text>
            <hr>
        </v-card>
    </v-col>
</template>


<script>
export default {
    name: "ArticleComp",
    props: {
        spec: Object
    },
    data() {
        return {

        }
    },
    methods: {
        addArticle(spec){
            this.$emit("artClick",spec)
        }
    }
}
</script>


<style scoped>

</style>