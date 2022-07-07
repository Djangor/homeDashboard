<template>
      <div class="topLeft">
        <DateTime></DateTime>
        <Weather></Weather>
        <div class="aqiWrapper">
          <AQIHome></AQIHome>
          <AQIOutside></AQIOutside>
        </div>
      </div>
      <div class="topRight">
        <BringItems></BringItems>
      </div>
      <div class="bottomLeft">
        <DjangoCal></DjangoCal>

      </div>
      <div class="bottomRight">
        <DjangoTasks></DjangoTasks>
      </div>
      <div class="rightCenter">
        <CryptoView></CryptoView>
      </div>
      <div class="bottomCenter">
      </div>
</template>

<script>
import AQIHome from "@/components/AQIHome";
import AQIOutside from "@/components/AQIOutside";
import DjangoCal from "@/components/DjangoCal";
import DjangoTasks from "@/components/DjangoTasks";
import BringItems from "@/components/Bring";
import Weather from "@/components/Weather";
import CryptoView from "@/components/Crypto";
import DateTime from "@/components/DateTime";
export default {
  name: 'App',
  components: {
    AQIHome,
    AQIOutside,
    DjangoCal,
    DjangoTasks,
    BringItems,
    Weather,
    CryptoView,
    DateTime
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

.topLeft{
  position:absolute;
  left:20px;
  display:flex;
  flex-wrap:wrap;
  max-width: 700px;
}

.topRight {
  position:absolute;
  right:20px;
  top:20px;
}

.bottomLeft {
  position:absolute;
  left:20px;
  bottom:20px;
}
.bottomRight {
  position:absolute;
  bottom:20px;
  right:20px
}

.rightCenter {
  position: absolute;
  right: 20px;
  top: 50%;
  transform: translate(0, -50%);
}

.bottomCenter {
  position:absolute;
  bottom:20px;
  left:50%;
  transform: translateX(-50%);
}

.AQI {
  width: 150px;
  margin-top: 5px;
  max-height: 60px;
}
.title {
  font-size: 50pt;
  margin: 0 auto;
  margin-top: 14px;
}
.unit{
  font-family: "Ubuntu Regular";
  margin: 0 auto;
  padding-left: 10px;
}
</style>
