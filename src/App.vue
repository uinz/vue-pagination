<template>
  <div id="app">
    <Pagination :current.sync="current" @change="handleChange" />
  </div>
</template>

<script>
import Pagination from "./components/Pagination";

export default {
  name: "App",
  components: {
    Pagination
  },
  data() {
    return {current: 1}
  },
  mounted() {

  },
  methods: {
    handleChange(page) {
      this.current = page
    }
  },
  watch: {
    current(v) {
      console.log(v)
    }
  }
};
</script>

