<script>
import Vue from "vue";

let handleClickOutside;

export default {
  bind(element, binding, vnode) {
    handleClickOutside = event => {
      event.stopPropagation();
      const { handler, exclude } = binding.value;
      let clickedOnExcluded = false;
      if (exclude) {
        exclude.forEach(excludedClassName => {
          if (!clickedOnExcluded) {
            clickedOnExcluded = event.target.classList.contains(excludedClassName);
          }
        })
      }
      if (!element.contains(event.target) && !clickedOnExcluded) {
        handler(event);
      }
    };
    document.addEventListener("click", handleClickOutside);
  },
  unbind(element) {
    document.removeEventListener("click", handleClickOutside);
  }
};
</script>