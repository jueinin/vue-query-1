<script lang="ts">
import { computed, defineComponent, PropType, h } from "vue-demi";

import type { Query } from "react-query/types";

import { useTheme } from "../useTheme";
import { getQueryState, getQueryStatusColor, QueryState } from "../utils";

export default defineComponent({
  name: "QueryItem",
  props: {
    query: {
      type: Object as PropType<Query>,
      required: true,
    },
  },
  setup(props, { emit }) {
    const theme = useTheme();
    // @ts-expect-error Accessing private property
    const observerCount = computed<number>(() => props.query.observers.length);
    const isStale = computed(
      () => getQueryState(props.query) === QueryState.Stale
    );
    const stateColor = computed(() => getQueryStatusColor(props.query, theme));

    const onQueryClick = () => {
      emit("selectQuery", props.query.queryHash);
    };

    return () => {
      const queryState = h(
        "div",
        {
          class: "query-state",
          style: {
            background: stateColor.value,
            textShadow: isStale.value ? "0" : "0 0 10px black",
            color: isStale.value ? "black" : "white",
          },
        },
        observerCount.value
      );

      const code = h("code", props.query.queryHash);

      return h(
        "div",
        {
          onClick: onQueryClick,
          style: {
            display: "flex",
            borderBottom: `solid 1px ${theme.grayAlt}`,
            cursor: "pointer",
          },
        },
        [queryState, code]
      );
    };
  },
});
</script>

<style scoped>
.query-state {
  align-items: center;
  display: flex;
  flex: 0 0 auto;
  font-weight: bold;
  height: 2rem;
  justify-content: center;
  width: 2rem;
}

code {
  font-size: 0.9em;
  padding: 0.5rem;
}
</style>
