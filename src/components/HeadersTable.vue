<template>
  <tr>
    <th
      class="table-header"
      v-for="name in headers"
      :key="name"
      @click="sorting(name)"
      :class="classForFields(name)"
    >
      {{ headersDescription[name] }}
      <span v-if="sortOptions.sortBy === name" class="sort-icon"></span>
    </th>
  </tr>
</template>

<script>
export default {
  props: {
    headers: Array,
    headersDescription: Object,
    sortOptions: Object,
  },
  methods: {
    classForFields(fieldName) {
      return {
        activeSort: fieldName === this.sortOptions.sortBy,
      };
    },
    sorting(sortName) {
      this.$emit('sorting', sortName);
    },
  },
};
</script>

<style lang="scss" scoped>
.table-header {
  position: relative;
  padding: 1rem 5px;
  padding-right: 1em;
  border-bottom: 2px solid rgb(128, 128, 128);
  cursor: pointer;

  &:not(:last-child):after {
    position: absolute;
    content: '';
    left: 100%;
    top: 1rem;
    bottom: 1rem;
    width: 2px;
    background-color: rgb(128, 128, 128);
  }
}
.sort-icon {
  position: absolute;
  display: block;
  width: 0.5em;
  height: 0.5em;
  right: 0.25em;
  top: calc(50% - 0.25em);
  border: solid #000;
  border-width: 3px 0 0 3px;
  transform: rotate(45deg);
}
</style>