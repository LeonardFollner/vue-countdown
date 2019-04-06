<template>
  <div id="app">
    <CountDown
      v-for="(countDown, index) in countDowns"
      :countDown="countDown"
      :key="countDown.timestamp"
      :onDelete="() => handleDelete(index)"
      @changed="handleCountDownChanged(index, $event)"
    />
    <div class="tile">
      <div @click="handleAddNewButtonClick" class="add-new-button">+</div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
import Cookies from "js-cookie";
import CountDown from "./components/CountDown";

const cookieName = "countDowns";

const timeOnSite = {
  timestamp: new Date().getTime(),
  title: "You entered this site"
};
const newYearCountDown = {
  timestamp: moment()
    .endOf("year")
    .valueOf(),
  title: "Time until New Year"
};

export default {
  name: "App",
  components: {
    CountDown
  },
  data: () => ({
    countDowns: []
  }),
  created() {
    const countDownsFromCookie = Cookies.getJSON(cookieName);
    if (!countDownsFromCookie) {
      this.countDowns.push(timeOnSite, newYearCountDown);
    } else {
      this.countDowns = countDownsFromCookie;
    }
  },
  methods: {
    handleAddNewButtonClick: function() {
      this.countDowns.push({
        timestamp: new Date().getTime(),
        title: "You added this Countdown"
      });
    },
    handleCountDownChanged: function(index, data) {
      this.countDowns[index] = data;
      this.persist();
    },
    handleDelete: function(index) {
      this.countDowns.splice(index, 1);
      this.persist();
    },
    persist: function() {
      Cookies.set(cookieName, this.countDowns);
    }
  }
};
</script>
<style>
* {
  box-sizing: border-box;
}
html,
body {
  height: 100%;
  min-height: 100%;
  margin: 0;
  padding: 0;
  font-size: 2vh;
}
#app {
  height: 100%;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  color: #2c3e50;
  display: flex;
  flex-wrap: wrap;
}
.add-new-button {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  width: 100%;
  height: 100%;
  font-size: 6rem;
}
.add-new-button:hover {
  background-color: deepskyblue;
}
</style>