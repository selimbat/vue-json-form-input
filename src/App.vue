<template>
  <div id="app">
    <form>
      <JsonFormInput :template="template" :labels="labels"> </JsonFormInput>
    </form>
  </div>
</template>

<script>
  import JsonFormInput from "./components/JsonFormInput";

  export default {
    name: "App",
    components: {
      JsonFormInput,
    },
    data() {
      return {
        template: {
          name: "Enter your name",
          age: 18,
          address: {
            road: "1 rue de la Baie",
            zip: 18,
          },
          hobbies: ["lasergame"],
          favoriteBeers: [
            {
              name: "Milky devil's",
              style: "Milk stout",
            },
          ],
          deep: [
            {
              deep: {
                lol: "lol",
                deep: {
                  deep: {
                    deep: {
                      name: "coucou",
                    },
                  },
                },
                except: 13,
              },
            },
          ],
        },
        labels: {
          name: "Name",
          age: "Age",
          address: "Address",
          road: "Road",
          zip: "Zip",
          hobbies: "Hobbies",
          favoriteBeers: "Favourite beers",
          style: "Style",
          deep: "Deep test",
        },
      };
    },
  };
</script>

<style>
  #app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    text-align: left;
  }
</style>
