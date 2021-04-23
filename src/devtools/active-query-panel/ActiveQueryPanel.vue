<script lang="ts">
import { defineComponent, PropType, h } from "vue-demi";

import type { Query } from "react-query/types";

import Explorer from "./Explorer.vue";
import InfoPanel from "./InfoPanel.vue";
import QueryActions from "./QueryActions.vue";
import QueryDetails from "./QueryDetails.vue";

export default defineComponent({
  name: "ActiveQueryPanel",
  components: { Explorer, InfoPanel, QueryActions, QueryDetails },
  props: {
    query: {
      type: Object as PropType<Query>,
      required: true,
    },
  },
  setup(props) {
    return () => {
      return h(
        "div",
        {
          class: "active-query-panel",
        },
        [
          h(QueryDetails, { query: props.query }),
          h(QueryActions, { query: props.query }),
          h(InfoPanel, { title: "Data Explorer" }, [
            h(Explorer, {
              label: "Data",
              value: props.query.state.data,
              defaultExpanded: {},
            }),
          ]),
          h(InfoPanel, { title: "Query Explorer" }, [
            h(Explorer, {
              label: "Query",
              value: props.query,
              defaultExpanded: { queryKey: true },
            }),
          ]),
        ]
      );
    };
  },
});
</script>

<style scoped>
.active-query-panel {
  display: flex;
  flex-direction: column;
  flex: 1 1 500px;
  height: 100%;
  overflow: auto;
}
</style>
