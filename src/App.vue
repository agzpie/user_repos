<template>
  <div class="main">
    <h2 class="header1">Whose repositories would you like to see?</h2>

    <div class="container">
      <div class="form-container">
        <label for="user-name">github @: </label>
        <input
          v-model="name"
          id="user-name"
          name="user-name"
          placeholder="github user name here"
        />
      </div>
      <div class="btn-container">
        <button
          class="accept-button"
          @click="
            userName = name;
            isActive = false;
          "
        >
          OK
        </button>
      </div>
    </div>

    <h4 :class="{ inactive: isActive }">
      The most popular repositories of {{ userName }}:
    </h4>

    <ListElement
      v-for="lib in orderedList"
      :key="lib.name"
      :repository-name="lib.name"
      :description="lib.description"
      :stars="lib.stargazers_count"
    >
    </ListElement>
  </div>
</template>

<script>
import ListElement from "./components/ListElement";
import { ref, reactive, toRefs, watchEffect, computed } from "vue";
export default {
  name: "App",

  components: {
    ListElement,
  },

  setup() {
    const name = ref(null);
    const userName = ref(null);
    const state = reactive({ data: [] });

    watchEffect(() => {
      if (userName.value) {
        fetch(`https://api.github.com/users/${userName.value}/repos`)
          .then((response) => response.json())
          .then((data) => {
            state.data = data;
            console.log("data", data);
            name.value = "";
          })
          .catch(err => console.log(err.message))
      }
    });

    const orderedList = computed(() => {
      return [...state.data].sort((a, b) => {
        return a.stargazers_count < b.stargazers_count ? 1 : -1;
      });
    });

    return {
      isActive: true,
      name,
      userName,
      ListElement,
      ...toRefs(state),
      orderedList,
    };
  },
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
  color: #2c3e50;
  margin-top: 60px;
}

.main {
  width: fit-content;
  margin: auto;
  background-color: white;
  padding: 0.6rem 1.5rem;
  border: 1px solid #2c3e50;
  border-radius: 10px;
  transition: 3s;
  transition-timing-function: ease;
  align-content: space-around;
}

.container {
  display: flex;
}

.form-container {
  align-self: flex-start;
  flex-grow: 1;
}
.btn-container {
  justify-content: center;
  flex-grow: 1;
}
.accept-button {
  align-self: center;
  color: white;
  font-weight: 600;
  background: rgb(34, 158, 94);
  background: radial-gradient(circle, #31a851 28%, rgba(31, 149, 52, 1) 78%);
  background-size: 100% auto;
  padding: 0.25rem 1.5rem;
  position: center;
  border: 1px solid #2c3e50;
  border-radius: 5px;
  transition: 0.3s all ease;
  cursor: pointer;
  box-shadow: 0 2px 10px rgb(217, 236, 220);
  /* TODO button background is too high on hover */
}
.accept-button:hover {
  background-size: 170%;
  background-position: center;
  box-shadow: 0 6px 16px rgb(202, 226, 205);
  transform: translate(0px, -1px);
  letter-spacing: 0.05rem;
}

.inactive {
  color: rgb(226, 226, 226);
  transition: 2s;
}

/* Extra small devices (phones, 450px and down) */
@media only screen and (max-width: 450px) {
  .container {
    flex-wrap: wrap;
    flex-direction: column;
    justify-content: space-evenly;
    height: 20vw;
    align-items: center;
  }
}

</style>
