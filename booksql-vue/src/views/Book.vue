<template>
    <div class="book container">
        <ApolloQuery :query="require('@/graphql/queries/Books.gql')" :variables="{id:$route.params.id}" >
        <template slot-scope="{ result: { data, loading }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <div v-else class="flex mt-16 flex-col lg:flex-row">
            <div>
              <img :src="`http://127.0.0.1:8000/img/${book.image}`" alt="cover image">
            </div>
            <div class="w-full lg:w-2/3 ml-0 mt-8 lg:mt-0 lg:ml-16">
              <div class="text-4xl font-bold">{{data.book.title}}</div>
              <div class="text-2xl text-grey-darkest mb-8">{{data.book.author}}</div>
              <div class="text-lg text-grey-darkest leading-normal">{{data.book.description}}</div>
              <div class="my-12">
                <a :href="data.book.link" target="_blank" class="border border-purple-dark
                border-solid rounded px-4 py-4 hover:bg-purple hover:text-white">ver link</a>
              </div>
              <router-link class="link-margin" :to="`/book/${data.book.id}/edit`">Editar</router-link>
              &middot;
              <router-link class="link-margin" v-on:click.prevent="deleteBook">Eliminar</router-link>
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>
</template>

<script>
import deleteBook from "@/graphql/mutations/DeleteBook.gql";

export default {
    name:'Book',
    data() {
        return {
            
        }
    },
    methods: {
        deleteBook(){
            this.$apollo.mutate({
              mutation:deleteBook,
              variables: {
                id:this.$route.params.id,
              },
            }).then((data) => {
              console.log(data)
              this.$router.push('/')
            }).catch((error) => {
              console.error(error)
            })
        }
    },
    
}
</script>