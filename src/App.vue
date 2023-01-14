<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
  <ul>
    <li v-for="post in posts" :key="post.id">
      {{ post.title }}
    </li>
  </ul>
  <button @click="handleMutation"> Mutation </button>
</template>
 
<script setup>
  import HelloWorld from './components/HelloWorld.vue' 
  import { onMounted, ref } from 'vue';

  const posts = ref([])

  onMounted(()=>{
    fetch('http://l.graphql.test/graphql', {
      method:'POST',
      headers:{
        'Content-Type':	'application/json'
      },
      body:JSON.stringify({
        query:`
          query{
            posts
            {
              paginatorInfo{
                count
                currentPage
                perPage
                total
                
              }
              data{
                id
                title
                body
              }
            }
          }
        `
      })
    })
    .then((res)=> res.json())
    .then((result)=>{
      console.log(result)
      posts.value = result.data.posts.data
    })

  })

  function handleMutation()
  {
    fetch('http://l.graphql.test/graphql', {
      method:'POST',
      headers:{
        'Content-Type':	'application/json'
      },
      body:JSON.stringify({
        query:`
        mutation{
          createPostResolver(
            user_id : "1",
            title : "Hello From Vue ",
            body : "content from Vue",
          ){
            title
            body
            id
          }
        }
        `
      })
    })
    .then(res=> res.json())
    .then(result=>{
      console.log(result)
     alert('post was created ~')
    })
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
