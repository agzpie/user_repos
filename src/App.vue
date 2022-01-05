<template>
      <div class="main">
        <h2 class="header">Whose repositories would you like to see?</h2>
        <form>
            <label for="user-name">github @: </label>
            <input v-model="name" type="text" id="user-name" name="user-name">
            <button class="accept-button" @click="userName = name">OK</button>
        </form>

        <h2 class="header">The most popular repositories of user: {{userName}}</h2> 
    </div>
</template>

<script>
//import UserInput from './components/UserInput';
import {ref, watch} from "vue";
export default {
  name: "App",
  components: {},

  setup() {
    const name = ref(null)
    const userName = ref(null)

    watch(()=> {
      fetch('https://api.github.com/users/octocat/repos')
      .then((response)=> response.json)
      .then((data)=> {
        console.log("data", data);
      });
    });

    return {
      name,
      userName,
    }
  }
};
</script>

<style>
body {
  background-color: whitesmoke;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
}

.main {
  max-width: 60vw;
  margin: auto;
  background-color: white;
  padding: 1rem;
  border: 1px solid thistle;
  border-radius: 10px;
}

.accept-button {
  background-color: thistle;
  margin: 0.5rem 2rem;
  padding: 0.5rem 2rem;
  border: 1px solid rgb(107, 85, 107);
  border-radius: 5px;
}

</style>
