<template>
  <tbody>
    <template v-if="database.length">
      <tr
        v-for="business in database"
        :class="{
          'state-OK': business.inspection_description === 'NO ACTION',
          'state-DANG':
            business.inspection_description === 'REINSPECTION REQUIRED',
          'state-GOOD': business.inspection_description === 'ISSUED PERMIT',
          'state-WARN':
            business.inspection_description === 'SHORTER DATE ADVANCE',
        }"
        :key="business._id.$oid"
      >
        <td class="body-field" v-for="name in colNames" :key="name">
          {{ business[name] }}
        </td>
      </tr>
    </template>
    <tr v-else>
      <td class="body-field" :colspan="colNames.length">Нет данных</td>
    </tr>
  </tbody>
</template>

<script>
export default {
  props: {
    colNames: Array,
    database: Array,
  },
};
</script>

<style lang="scss" scoped>
.body-field {
  word-break: break-all;
  position: relative;
  padding: 1rem 5px;
  border-bottom: 1px solid rgb(128, 128, 128);

  &:not(:last-child):after {
    position: absolute;
    content: '';
    left: 100%;
    top: 1rem;
    bottom: 1rem;
    width: 1px;
    background-color: rgb(128, 128, 128);
  }
}
.state-OK {
  background-color: #9dd8a2;
}
.state-WARN {
  background-color: #fff692;
}
.state-DANG {
  background-color: #ffa800;
}
.state-GOOD {
  background-color: #2bb22b;
}
</style>