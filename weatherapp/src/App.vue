<script>
import TimesDisplayContainerVue from "./TimesDisplayContainer.vue"
import axios from "axios"
export default {
  components: {
    TimesDisplayContainerVue
  },
  data() {
    return {
      fetchedData: [],
      startFlg: ""
    }
  },
  methods: {
    async gettemp() {
      const url = "https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&hourly=temperature_2m"
      const response = await axios.get(url)
      const tempArr = response.data.hourly.temperature_2m
      const timeArr = response.data.hourly.time
      // let tempTimeArr = []
      // for (let i = 0; i < tempArr.length; i++) {
      //   tempTimeArr.push({
      //     temp: tempArr[i],
      //     time: timeArr[i]
      //   })
      // }
      const tempTimeArr = tempArr.map((_, index) => {
        return {
          temp: tempArr[index],
          time: timeArr[index]
        }
      })
      this.fetchedData = tempTimeArr
      this.startFlg = true
      this.hotFlg = true
    },
    hotFil() {
      const filteredData = this.fetchedData.filter(function (tempTimeObj) {
        return tempTimeObj.temp > 15
      });
      alert(JSON.stringify(filteredData))
    }
  },

}

</script>
<template>
  <div class="container">
    <div v-if="!startFlg">
      <div class="title">
        <h1 class="Btntitle">I know u want the temperature...press this</h1>
      </div>
      <button @click="gettemp" class="Btn srttemp">show temperature</button>
    </div>
    <div v-if="startFlg">
      <button @click="hotFil" class="Btn srtHot">Filter only Hot time</button>
      <TimesDisplayContainerVue :Data="fetchedData">
      </TimesDisplayContainerVue>
    </div>

  </div>
</template>
<style>
* {
  text-align: center;
}

li {
  list-style: none;
}

.Btn {
  box-shadow: 0px 2px rgb(109, 108, 108);
  cursor: pointer;
}

.Btn:active {
  position: relative;
  top: 2px;
  box-shadow: none;
}

ul {
  filter: drop-shadow(1px 2px 1px lightgreen);
}
.Btntitle{
  color: fuchsia;
}



</style>