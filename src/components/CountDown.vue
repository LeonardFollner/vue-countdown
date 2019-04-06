<template>
  <div class="tile">
    <TitleInput :value="countDown.title" :onTitleChanged="onTitleChanged"/>
    <div class="timer" @click="openDatePicker">{{ timer }}</div>
    <Datepicker
      input-class="hidden"
      wrapper-class="date-picker"
      ref="datepicker"
      @selected="onDateChanged"
      value="date"
      v-onClickOutside="{
        handler: closeDatePicker,
        exclude: ['timer']
        }"
    />
  </div>
</template>

<script>
import Vue from "vue";
import moment from "moment";
import Datepicker from "vuejs-datepicker";

import onClickOutside from '../directives/onClickOutside';
import TitleInput from "./TitleInput";

export default {
  name: "CountDown",
  components: {
    Datepicker,
    TitleInput
  },
  props: {
    countDown: {
      title: String,
      timestamp: Number
    }
  },
  data() {
    return {
      timer: ""
    };
  },
  computed: {
    date: function() {
      return new Date(this.countDown.timestamp);
    }
  },
  created() {
    this.setTimer();
    setInterval(this.setTimer, 500);
  },
  destroyed() {
    clearInterval(this.setTimer);
  },
  directives: {
    onClickOutside: onClickOutside,
  },
  methods: {
    setTimer() {
      this.timer = moment(this.countDown.timestamp).fromNow();
    },
    openDatePicker: function() {
      this.$refs.datepicker.showCalendar();
    },
    closeDatePicker: function() {
      this.$refs.datepicker.close();
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
    }
  }
};
</script>

<style>
.hidden {
  display: none;
}
.tile {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  width: 100%;
  flex-grow: 1;
  min-height: 20vh;
  max-height: 100%;

  border: 1px solid black;
}
@media (min-width: 500px) {
  .tile {
    width: 50%;
  }
}
@media (min-width: 700px) {
  .tile {
    width: 33%;
  }
}
@media (min-width: 1300px) {
  .tile {
    width: 25%;
  }
}
@media (min-width: 1700px) {
  .tile {
    width: 20%;
  }
}
.timer {
  padding: 18px;
}
.date-picker {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 9px;
}
</style>
<style scoped>
.timer {
  font-size: 1.5rem;
  cursor: pointer;
}
.timer:hover {
  background-color: lightblue;
}
</style>