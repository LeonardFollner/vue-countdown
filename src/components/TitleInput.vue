<template>
  <div
    class="input-wrapper"
    @click="enterEditMode"
    v-onClickOutside="{
      handler: exitEditMode,
      exclude: [
        /*
          * necessary because at the time onClickOutside handles the click
          * .title is not mounted anymore
          * and thus element.contains(.title) returns false
          */
        'title', 
      ]
    }"
  >
    <div class="title" v-if="!inEditMode">{{value}}</div>
    <input
      v-if="inEditMode"
      type="text"
      :value="value"
      placeholder="Lorem Ipsum"
      @keydown="onTitleChanged"
      @keyup.13="submit"
      autofocus
      v-focus
    >
  </div>
</template>

<script>
import focus from "../directives/focus";
import onClickOutside from "../directives/onClickOutside";

export default {
  name: "TitleInput",
  props: {
    value: String,
    onTitleChanged: Function
  },
  data() {
    return {
      inEditMode: false
    };
  },
  directives: {
    onClickOutside,
    focus
  },
  methods: {
    enterEditMode: function() {
      this.inEditMode = true;
    },
    exitEditMode: function() {
      this.inEditMode = false;
    },
    submit: function(event) {
      this.onTitleChanged(event);
      this.exitEditMode();
    }
  }
};
</script>

<style scoped>
.title {
  overflow: hidden;
}
.title,
input {
  font-size: 3rem;
  text-align: center;
}
input {
  width: 100%;
  border: none;
  background: none;
  color: #2c3e50;
}
.input-wrapper {
  width: 100%;
  padding: 18px;
  text-align: center;
  cursor: text;
}
.input-wrapper:hover {
  background-color: deepskyblue;
}
</style>
