<template>
  <div id="appContainer">
    <form @submit.prevent="getDefinitions()">
      <input placeholder="my word" type="text" v-model="word">
      <button  class="btn waves-effect waves-light" type="submit">Get definition</button>
    </form>
    <p v-if="!validQuery">Error: The word you are searching for does not exist. Please try a different word.</p>
    <div id="cardContainer" v-for="definition in definitionsArray" :key="definition">
      <div class="colunm">
        <div class="col s12 m5">
          <div class="card-panel teal">
            <div class="white-text">{{definition}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import {ref} from "vue";
export default {
  name: 'App',
  setup(){
    const word = ref("");
    const validQuery = ref(true)
    const definitionsArray = ref([]);
    async function getDefinitions(){
      definitionsArray.value.splice(0, definitionsArray.value.length)
     try {
      const url = "https://api.dictionaryapi.dev/api/v2/entries/en/" + word.value
      const {data} = await axios.get(url);
      data.map(({meanings}) =>{
        meanings.map(({definitions}) =>{
          definitions.forEach(({definition}) => {
            if(typeof definition === "string"){
              definitionsArray.value.push(definition);
            }
          });
        })
      })
      } catch(error) {
        validQuery.value = false;
      }
    
    }
    return {word, getDefinitions, definitionsArray, validQuery}
  }
}
</script>

<style>
#appContainer{
  margin: 10px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
