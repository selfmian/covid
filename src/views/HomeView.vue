<template>
  <div class="home">
    <div class="chinaMap" ref="chinaMap"></div>
  </div>
</template>

<script>
// @ is an alias to /src
import * as echarts from "echarts";
import china from "@/assets/china.json";
import chinaCovid from "@/assets/chinaCovid.json";
console.log(china);
console.log(chinaCovid);
let nameMap = {};
china.features.forEach((item) => {
  let n = item.properties.name;
  nameMap[n] = item.properties.name.replace(/(市|省|自治区|特别行政区|壮族|回族|维吾尔)/g,"");
});
console.log(nameMap);
export default {
  name: "HomeView",
  components: {},
  mounted() {
    let chinaMap = echarts.init(this.$refs.chinaMap);
    // 注册地图：第一个参数是地图名称，第二个参数是地图数据
    echarts.registerMap("china", china);
    let option = {
      title: {
        text: "中国疫情地图",
      },
      tooltip: {
        trigger: "item",
        formatter: "{b}<br/>确诊人数{c}",
      },
      visualMap: {
        min: 0,
        max: 5000000,
        text: ["High", "Low"],
        realtime: false,
        calculable: true,
        inRange: {
          color: ["lightcoral", "red", "darkred"],
        },
      },
      series: {
        type: "map",
        map: "china",
        data: chinaCovid.data.areaTree[2].children.map((item) => {
          return {
            name: item.name,
            value: item.total.confirm,
          };
        }),
        nameMap,
      },
    };
    chinaMap.setOption(option);
  },
};
</script>

<style scoped lang="scss">
.chinaMap {
  border: 1px solid black;
  width: 600px;
  height: 500px;
}
</style>