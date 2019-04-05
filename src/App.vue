<template>
  <div id="app">
    <CountDown
      v-for="(countDown, index) in countDowns"
      v-bind:countDown="countDown"
      v-bind:key="countDown.timestamp"
      @changed="handleCountDownChanged(index, $event)"
    />
    <button v-on:click="handleAddNewButtonClick">+</button>
  </div>
</template>

<script>
import Cookies from 'js-cookie';
import CountDown from "./components/CountDown";

const cookieName = 'countDowns';

const initialCountDown = {
  timestamp: new Date().getTime(),
  title: "You entered this site",
};

export default {
  name: "App",
  components: {
    CountDown,
  },
  data: () => ({
    countDowns: [],
  }),
  created () {
    const countDownsFromCookie = Cookies.getJSON(cookieName);
    if (!countDownsFromCookie) {
      this.countDowns.push(
        initialCountDown,
      );
    } else {
      this.countDowns = countDownsFromCookie;
    }
  },
  methods: {
    handleAddNewButtonClick: function() {
      this.countDowns.push({
        timestamp: new Date().getTime(),
        title: "You added this Countdown",
      });
    },
    handleCountDownChanged: function(index, data) {
      this.countDowns[index] = data;
      Cookies.set(cookieName, this.countDowns);
    }
  }
};
</script>
