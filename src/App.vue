<script setup>
import {computed, onMounted, ref} from "vue";
import BlogPost from "./components/BlogPost.vue";
import PaginatePost from "./components/PaginatePost.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";


const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const find = ref(postXpage);
const loading = ref(true);
const favorito = ref("");

const cambiarFavorito = (title) => {
  favorito.value = title;
};

const next = () => {
  inicio.value = inicio.value + postXpage
  find.value = find.value + postXpage
}

const prev = () => {
  //inicio.value = inicio.value - postXpage
  inicio.value += -postXpage;
  find.value += -postXpage;
}

onMounted(async() =>{
  //loading.value = true
  try{
    const res = await fetch("https://jsonplaceholder.typicode.com/posts")
    posts.value = await res.json();
  }catch (error){
    console.log(error);
  } finally {
    setTimeout(() =>{
     loading.value = false;
     },2000);
  }
});
//fetch("https://jsonplaceholder.typicode.com/posts")
//.then((res) => res.json())
//  .then((data) => {
//    posts.value = data
//  })
//  .catch((e) => console.log(e))
//  .finally(() => {
//    setTimeout(() =>{
//     loading.value = false
//    },2000);
//  });


  const maxLength = computed(() => posts.value.length)
</script>

<template>
  <div>
  <LoadingSpinner v-if="loading"/>
  <div class="container" v-else>
    <h1>APP</h1>
    <h2>Mis Post Favorito: {{ favorito }}</h2> 
    
    <PaginatePost
     @next="next"
      @prev="prev" 
      :inicio="inicio" 
      :find="find"
      :maxLength="maxLength"
      class="mb-2"
      />
    
    <BlogPost 
      v-for="post in posts.slice(inicio, find)"
      :key="post.id"
      :title="post.title" 
      :id="post.id" 
      :body="post.body" 
      :cambiarFavorito="cambiarFavorito"
      class="mb-2"
    ></BlogPost>
  </div>
  </div>
</template>

<style>
  
</style>
