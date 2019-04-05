<template>
  <div>
    <input class="title" type="text" :value="countDown.title" placeholder="Lorem Ipsum" @change="onTitleChanged">
    <Datepicker input-class="hidden" ref="datePickerIsOpen" @selected="onDateChanged" value="date" />
    <div class="timer" @click="openDatePicker">{{ timer }}</div>
  </div>
</template>

<script>
import Vue from 'vue';
import moment from "moment";
import Datepicker from "vuejs-datepicker";

export default {
  name: "CountDown",
  components: {
    Datepicker
  },
  props: {
    countDown: {
      title: String,
      timestamp: Number,
    },
  },
  data () {
    return {
      timer: '',
    }
  },
  computed: {
    date: function() {
      return new Date(this.countDown.timestamp);
    }
  },
  created () {
    this.setTimer();
    setInterval(this.setTimer, 500)
  },
  destroyed () {
    clearInterval(this.setTimer)
  },
  methods: {
    setTimer () {
      this.timer = moment(this.countDown.timestamp).fromNow();
    },
    openDatePicker: function() {
      this.$refs.datePickerIsOpen.showCalendar();
    },
    onDateChanged: function(newDate) {
      const newTimestamp = newDate.getTime();
      this.countDown.timestamp = newTimestamp;
      this.$emit("changed", this.countDown);
    },
    onTitleChanged: function(event) {
      const newTitle = event.target.value;
      this.countDown.title = newTitle;
      this.$emit("changed", this.countDown);
    },
    onClose: function() {
      console.log('close');
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.hidden {
  display: none;
}
</style>
<style scoped>
  .title {
    border: none;
    font-size: 3rem;
  }
  .timer {
    font-size: 1.5rem;
    cursor: pointer;
  }
</style>