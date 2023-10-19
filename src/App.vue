<template>
  <div class="app">
    <div class="header container h-100 p-5">
      <h1 class="mb-5">Weather App</h1>
      <div class="d-flex justify-content-center h-100">
        <div class="searchbar w-50 mx-2">
          <input type="text" class="input form-control" placeholder="Enter a City">
        </div>
        <button class="btn-serch btn btn-primary">Search <i class="fas fa-search"></i></button>
      </div>
    </div>
    <MyWeather />
  </div>
</template>

<script>
import MyWeather from './components/Weather.vue';

export default {
  name: 'App',
  components: {
    MyWeather
  },

  data() {
    return {
      city: '',
      showWeather: false
    }
  },
  methods: {
    async searchWeather() {
      this.showWeather = false;
      await this.$nextTick();
      this.showWeather = true;
    }
  }
}
</script>

<style>
body {
  background-color: #121212 !important;

}

.header {
  background-color: #212730;
  border-radius: 20px;
  color: #fff;
  text-align: center;
  align-items: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  margin: 5rem;
}


</style>
