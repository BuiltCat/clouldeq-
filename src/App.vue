<template>
  <div id="app"  v-loading="loading" element-loading-text="数据正在计算处理">
    <gis-header loading="loading" @changeLoading = "changeLoading"></gis-header>  
    <router-view ref="hollow"/>
  </div>
</template>

<script>
const axios = require("axios");
import header from "../src/components/header";
export default {
  name: "app",
  data (){
    return {
      loading: false
    }
  },
  mounted() {
    this.$refs.hollow.initChart();
    const dataView = "http://47.101.35.46/ceServer_php/public/dataView";
    const getLevelData =
      "http://47.101.35.46/ceServer_php/public/getLevelData";

    const groupByMg = "http://47.101.35.46/ceServer_php/public/groupByMg";
    
      const getEarthquakeNumber =
        "http://47.101.35.46/ceServer_php/public/getEarthquakeNumber";
    axios
      .get(dataView)
      .then(
        function(response) {
          if (response.data.reason === "Success") {
            this.$store.state.timelinedata = response.data.result;
            console.log(response.data.result);

          } else {
            throw new Error("数据请求失败");
          }
        }.bind(this)
      )
      .catch(function(error) {
        console.log(error);
      });
    axios
      .get(getLevelData)
      .then(
        function(response) {
          if (response.data.reason === "Success") {
            this.$store.state.chartdata = response.data.result;
          } else {
            throw new Error("数据请求失败");
          }
        }.bind(this)
      )
      .catch(function(error) {
        console.log(error);
      });
    axios
      .get(groupByMg)
      .then(
        function(response) {
          if (response.data.reason === "Success") {
            this.$store.state.gradeData = response.data.result;
          } else {
            throw new Error("数据请求失败");
          }
        }.bind(this)
      )
      .catch(function(error) {
        console.log(error);
      });
      axios
        .get(getEarthquakeNumber)
        .then(
          function(response) {
            if (response.data.reason === "Success") {
              this.$store.state.linedata = response.data.result;
            } else {
              throw new Error("数据请求失败");
            }
          }.bind(this)
        )
        .catch(function(error) {
          console.log(error);
        });
  },
  components: {
    "gis-header": header
  },
  methods: {
    changeLoading () {
      if(this.loading){
        this.loading = false;
      }else{
        this.loading = true;
      }
    }
  }
};
</script>

<style>
#app {
  height: 100%;
  width: 100%;
}
</style>
