<script lang="ts">
import { defineComponent, h } from "vue-demi";

import { useTheme } from "../useTheme";

export default defineComponent({
  name: "InfoPanel",
  props: {
    title: {
      type: String,
      required: true,
    },
  },
  setup(props, { slots }) {
    const theme = useTheme();

    return () => {
      return h("div", [
        h(
          "div",
          {
            class: "title",
            style: {
              background: theme.backgroundAlt,
            },
          },
          props.title
        ),
        h("div", { class: "panel" }, slots),
      ]);
    };
  },
});
</script>

<style scoped>
.title {
  padding: 0.5rem;
  position: sticky;
  top: 0;
  z-index: 1;
}

.panel {
  padding: 0.5rem;
}
</style>
