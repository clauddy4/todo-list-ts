<template>
  <aside class="app-filters">
    <section class="toggle-group">
      <button
        v-for="filter in filters"
        :key="filter"
        class="button"
        :class="{ 'button--primary': activeFilter === filter }"
        @click="setFilter(filter)"
      >
        {{ filter }}
      </button>
    </section>
  </aside>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { PropType } from 'vue';
import { Filter } from '@/types/Filter';

interface State {
  filters: Filter[];
}

export default defineComponent({
  props: {
    activeFilter: {
      type: String as PropType<Filter>,
      required: true,
    },
  },
  data: (): State => ({
    filters: ['All', 'Active', 'Done'],
  }),
  methods: {
    setFilter(filter: Filter) {
      this.$emit('setFilter', filter);
    },
  },
  emits: {
    setFilter: (filter: Filter) => filter,
  },
});
</script>
