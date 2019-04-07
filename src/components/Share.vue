<template>
  <div @click="onShareClick">
    {{label}}
    <input
      :class="{hidden: !inputIsVisible}"
      ref="inputRef"
      :value="generateUrl()"
    >
  </div>
</template>

<script>
const originalLabel = "Share";

export default {
  name: "Share",
  data() {
    return {
      label: originalLabel,
      inputIsVisible: false,
    };
  },
  props: {
    countDown: Object
  },
  methods: {
    generateUrl: function() {
      const url = new URL(window.location.href);
      const parameter = url.searchParams;
      parameter.set('x', this.countDown.timestamp);
      parameter.set('y', this.countDown.title);
      parameter.set('z', this.countDown.uuid);
      return url.href;
    },
    onShareClick: function() {
      this.inputIsVisible = true;
      window.setTimeout(this.copyToClipboard, 100);
    },
    copyToClipboard: function() {
      this.$refs.inputRef.select();
      document.execCommand("copy");
      this.label = "Copied to clipboard";
      window.setTimeout(this.reset, 2200);
    },
    reset: function() {
      this.inputIsVisible = false;
      this.label = originalLabel;
    },
  }
};
</script>
