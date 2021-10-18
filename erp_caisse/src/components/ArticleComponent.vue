<template>
    <div> 
        <p> {{ spec.name }}  </p>
        <p> {{ spec.prix }}€  </p>
        <button
        @click="addArticle(spec)">Ajouter</button>
        <hr>
    </div>
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