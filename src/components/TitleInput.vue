<template>
  <div class="input-wrapper" @click="enterEditMode">
    <div class="title" v-if="!inEditMode">{{value}}</div>
    <input
      v-if="inEditMode"
      type="text"
      :value="value"
      placeholder="Lorem Ipsum"
      @change="onTitleChanged"
      @keyup.13="submit"
      autofocus
    >
  </div>
</template>

<script>
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
  methods: {
    enterEditMode: function() {
      this.inEditMode = true;
    },
    exitEditMode: function() {
      this.inEditMode = false;
    },
    submit: function(event) {
      const newTitle = event.target.value;
      this.onTitleChanged(newTitle);
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
}
.input-wrapper {
  width: 100%;
  padding: 18px;
  text-align: center;
  cursor: text;
}
.input-wrapper:hover {
  background-color: lightblue;
}
</style>
