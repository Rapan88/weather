<template>

<div class="container">
  <div class="row">
    <div class="col-2" >x</div>
     <div class="col-8" >
       <WeatherModal/>
     </div>
      <div class="col-2">x</div>
  </div>
</div>
</template>

<script>
import WeatherModal from "./components/WeatherModal.vue";

export default {
  name: "App",
  components: { WeatherModal },
};
</script>

<style>

</style>
