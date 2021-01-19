<template>
  <div id="vue-app">
    <div class="component-container" v-for="(component, index) in components" :key="index">
      <Weather></Weather>
    </div>
    <h1 id="add-card-btn" @click="addWeather">Add Card</h1>
    <h1 id="reset-card-btn" @click="reset">reset</h1>
  </div>
</template>

<script>
import Weather from '../components/Weather.vue'

export default {
  data() {
    return {
      components: [Weather],
      count: 0
    }
  },
  components: {
    Weather
  },
  methods:{
    addWeather() {
      if (this.count < 2){
      this.components.push(Weather);
      this.count++}
    },
    reset() {
    this.components.splice(1);
    this.count = 0;
    console.log("splicing");
  }
  },
}

</script>

<style>
  #vue-app{
    display:flex;
    justify-content: center;
    flex-wrap:wrap;
  }
  #add-card-btn{
    position: absolute;
    bottom: 50px;
    color: #fff;
    background-color: rgba(0, 0, 0, 0.521);
    border: 3px solid #fff;
    padding:10px 25px;
    border-radius: 50px;
    font-size:32px;
    transition: all 0.2s ease-in-out;
    cursor: pointer;
  }
  #reset-card-btn{
    position: absolute;
    background-color: rgba(0, 0, 0, 0.521);
    bottom: 250px;
    color: #fff;
    border: 3px solid #fff;
    padding:10px 25px;
    border-radius: 50px;
    font-size:32px;
    transition: all 0.2s ease-in-out;
    cursor: pointer;
  }
  #add-card-btn:hover{
    font-size:90%;
  }

  #reset-card-btn:hover{
    font-size:90%;
  }
  @media only screen and (max-width: 600px) {
    #add-card-btn{
        bottom:1%;
        font-size: 20px;
    }
    #reset-card-btn{
          bottom: 15%;
          font-size: 20px;
    }
  }

</style>

