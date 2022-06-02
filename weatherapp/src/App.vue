<script>
import axios from "axios"
export default {
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
      const filteredData = this.fetchedData.filter(function (ondo) {
        return ondo.temp > 15
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
        <h1>I know u want the temperature...press this</h1>
      </div>
      <button @click="gettemp" class="Btn srttemp">show temperature</button>
    </div>
    <div v-if="startFlg">
      <button @click="hotFil">Filter only Hot time</button>
      <ul v-for="(info, index) in fetchedData" :key="index">
        <li>
          時間:{{ info.time }}温度:{{ info.temp }}{{ info.temp >= 15 ? "熱い" : "寒い" }}
        </li>
      </ul>

    </div>

  </div>
</template>