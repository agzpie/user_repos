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
          required
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

    <h4 v-if="success == true">
      The most popular repositories of {{ userName }}:
    </h4>
    <h4 v-else-if="success == false" class="error-message">
      Oopsie,<br />there was an error,<br />
      but don't worry and try again!
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
    let success = ref(null);
    const userNameValidator = /^[a-z\d](?:[a-z\d]|-(?=[a-z\d])){0,38}$/i;
    const split1 = reactive({ spl1: [] });
    const split2 = reactive({ spl2: [] });

    /*
     * Check for input in the form and then fetch data
     */
    async function myFetch(url) {
      let hasNext = false;

      do {
        let response = await fetch(url);
        if (!response.ok) {
          success.value = false;
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        success.value = true;
        // check response.headers for Link to get next page url
        split1.spl1 = response.headers.get("Link").split(",");
        let j = 0;
        while (j < split1.spl1.length) {
          split2.spl2[j] = split1.spl1[j].split(";");
          console.log(split2.spl2[j][0]);
          console.log(split2.spl2[j][1]);
          if (split2.spl2[j][1].includes("next")) {
            let urlNext = split2.spl2[j][0].replace(/[<>(\s)*]/g, "");
            console.log(urlNext);
            url = urlNext;
            hasNext = true;
            break;
          } else {
            hasNext = false;
          }
          j++;
        }

        // second .then
        let myData = await response.json();
        state.data.push(...myData);
        console.log("data", myData);
        name.value = "";
      } while (hasNext);
    }

    watchEffect(() => {
      if (!userName.value) return;

      if (!userNameValidator.test(userName.value)) {
        console.log("Username has invalid characters");
        return;
      }
      state.data = [];
      let url = `https://api.github.com/users/${userName.value}/repos?per_page=20`; // 20 on page just for dev & testing

      myFetch(url).catch((err) => {
        if (err.status == 404) {
          console.log("User not found");
        } else {
          console.log(err.message);
          console.log("oh no (internet probably)!");
        }
      });
    });

    // Sort list by star count
    const orderedList = computed(() => {
      if (state.data == 0) {
        return [];
      }
      console.log("Repo count = " + state.data.length);
      return [...state.data].sort((a, b) => {
        return a.stargazers_count < b.stargazers_count ? 1 : -1;
      });
    });

    return {
      success,
      isActive: true,
      name,
      userName,
      ListElement,
      ...toRefs(state),
      orderedList,
      myFetch,
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
  padding-top: 0.4rem;
  padding-bottom: 0.8rem;
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
.error-message {
  color: rgb(255, 0, 0);
  text-decoration: underline;
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
