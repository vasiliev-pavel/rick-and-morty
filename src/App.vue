<template>
  <div id="app">
    <CharacterList />
  </div>
</template>

<script>
import CharacterList from "./components/CharacterList.vue";

export default {
  name: "App",
  components: {
    CharacterList,
  },
};
</script>

<style>
body {
  background-color: rgb(39, 43, 51);
  color: #fff;
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}
</style>
