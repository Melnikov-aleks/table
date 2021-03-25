<template>
  <v-menu
    transition="scale-transition"
    offset-y
    min-width="auto"
    :disabled="!allowedDates.length"
  >
    <template v-slot:activator="{ on, attrs }">
      <input
        type="search"
        class="search-input"
        :class="classes"
        placeholder="Дата инспекции"
        readonly
        v-model="date"
        v-bind="attrs"
        v-on="on"
      />
    </template>
    <v-date-picker
      v-model="date"
      :allowed-dates="checkAllow"
      :show-current="date || allowedDates[0]"
      @change="selectDate"
      no-title
      scrollable
    >
    </v-date-picker>
  </v-menu>
</template>

<script>
export default {
  props: {
    allowedDates: Array,
    classes: { type: String, default: '' },
  },
  data() {
    return {
      date: '',
    };
  },
  methods: {
    selectDate(date) {
      this.$emit('selectDate', date);
    },
    checkAllow(dateToCheck) {
      return this.allowedDates.some((date) => date === dateToCheck);
    },
  },
};
</script>

<style lang="scss" scoped>
</style>