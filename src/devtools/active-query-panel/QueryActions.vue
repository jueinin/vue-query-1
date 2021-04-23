<script lang="ts">
import { computed, defineComponent, PropType, h } from "vue-demi";

import type { Query } from "react-query/types";

import InfoPanel from "./InfoPanel.vue";

import { useTheme } from "../useTheme";
import { useQueryClient } from "../../useQueryClient";

export default defineComponent({
  name: "QueryActions",
  components: { InfoPanel },
  props: {
    query: {
      type: Object as PropType<Query>,
      required: true,
    },
  },
  setup(props) {
    const theme = useTheme();
    const queryClient = useQueryClient();
    const isFetching = computed(() => props.query.state.isFetching);

    const doFetch = () => {
      props.query.fetch();
    };
    const doInvalidate = () => {
      queryClient.invalidateQueries(props.query);
    };
    const doReset = () => {
      queryClient.resetQueries(props.query);
    };
    const doRemove = () => {
      queryClient.removeQueries(props.query);
    };

    return () => {
      return h(InfoPanel, { title: "Actions" }, [
        h(
          "button",
          {
            type: "button",
            disabled: isFetching.value,
            style: {
              background: isFetching.value ? theme.grayAlt : theme.active,
              cursor: isFetching.value ? "not-allowed" : "pointer",
            },
            onClick: doFetch,
          },
          "Refetch"
        ),
        h(
          "button",
          {
            type: "button",
            style: {
              background: theme.warning,
              color: theme.inputTextColor,
            },
            onClick: doInvalidate,
          },
          "Invalidate"
        ),
        h(
          "button",
          {
            type: "button",
            style: {
              background: theme.gray,
            },
            onClick: doReset,
          },
          "Reset"
        ),
        h(
          "button",
          {
            type: "button",
            style: {
              background: theme.danger,
            },
            onClick: doRemove,
          },
          "Remove"
        ),
      ]);
    };
  },
});
</script>

<style scoped>
button {
  appearance: none;
  border-radius: 0.3em;
  border: 0;
  color: white;
  cursor: pointer;
  font-size: 0.9em;
  font-weight: bold;
  padding: 0.5em;
}

button:not(:last-of-type) {
  margin-right: 5px;
}
</style>
