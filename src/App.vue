<template>
  <div class="container" style="margin-left: 20px;">
    <div class="row">
      <div class="col" style="min-width:700px">
        <AQIHome></AQIHome>
        <AQIOutside></AQIOutside>
      </div>
      <div class="col">
        <DjangoCal></DjangoCal>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <BringItems></BringItems>
      </div>
      <div class="col">
        <DjangoTasks></DjangoTasks>
      </div>
      <div class="col">
        <Weather></Weather>
      </div>
    </div>
  </div>
</template>

<script>
import AQIHome from "@/components/AQIHome";
import AQIOutside from "@/components/AQIOutside";
import DjangoCal from "@/components/DjangoCal";
import DjangoTasks from "@/components/DjangoTasks";
import BringItems from "@/components/Bring";
import Weather from "@/components/Weather";

export default {
  name: 'App',
  components: {
    AQIHome,
    AQIOutside,
    DjangoCal,
    DjangoTasks,
    BringItems,
    Weather
  }
}
</script>

<style>
@font-face {
  font-family: "Bebas Neue";
  src: local("Bebas Neue"),
  url(/src/fonts/BebasNeue-Regular.ttf) format("truetype");
  font-family: "Ubuntu Regular";
  src: local("Ubuntu Regular"),
  url(/src/fonts/Ubuntu-Regular.ttf) format("truetype");
  font-family: "Ubuntu Bold";
  src: local("Ubuntu Bold"),
  url(/src/fonts/Ubuntu-Bold.ttf) format("truetype");
}
#app {
  font-family: "Ubuntu Bold";
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #FFFFFF;
  margin-top: 20px;
}

.AQI {
  margin-right: 15px;
  height: 150px;
  width: 150px;
  border-radius: 50%;
  display: inline-block;
}
.title {
  font-size: 50pt;
  margin: 0 auto;
  display: table;
  margin-top: 21px;
}
.unit{
  font-family: "Ubuntu Regular";
  display: table;
  margin: 0 auto;
  margin-top: -23px;
}
.BgGreen {
  background-color: forestgreen;
}

.BgOrange {
  background-color: darkorange;
}

.BgRed {
  background-color: crimson;
}
</style>
