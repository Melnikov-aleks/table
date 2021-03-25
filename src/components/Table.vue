<template>
  <table class="table">
    <caption>
      <div class="table-settings-list">
        <h3 class="table-title">Инспекция по ресторанам</h3>
        <app-dropdown class="table-settings"
          ><div
            v-for="(value, name) in editableFields"
            :key="name"
            class="dropdown-item"
            @click="toggleField(name)"
          >
            <span
              class="state-icon"
              :class="value ? 'icon-checked' : 'icon-close'"
            ></span>
            {{ fieldsDescr[name] }}
          </div></app-dropdown
        >
        <input
          type="text"
          class="search-input table-settings"
          placeholder="Поиск по полям таблицы"
          v-model="searchQuery"
        />
        <app-date-picker
          :allowedDates="datesCollection"
          classes="table-settings"
          @selectDate="setDateFilter"
        ></app-date-picker>
      </div>
    </caption>
    <thead>
      <app-headers
        :headers="fieldHeadersForShow"
        :headersDescription="fieldsDescr"
        :sortOptions="sortOptions"
        @sorting="setSort($event)"
      ></app-headers>
    </thead>
    <app-body
      :colNames="fieldHeadersForShow"
      :database="sortedDatabase"
    ></app-body>
    <tfoot>
      <tr>
        <td class="table-footer" :colspan="fieldHeadersForShow.length">
          Всего элементов : {{ sortedDatabase.length }}
        </td>
      </tr>
    </tfoot>
  </table>
</template>

<script>
import AppDropdown from '@/components/Dropdown.vue';
import AppHeaders from '@/components/HeadersTable.vue';
import AppBody from '@/components/BodyTable.vue';
import AppDatePicker from '@/components/DatePicker.vue';

export default {
  components: { AppDropdown, AppHeaders, AppDatePicker, AppBody },
  props: {
    database: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  data() {
    return {
      sortOptions: {
        sortBy: '',
      },
      sortedBy: '',
      searchQuery: '',
      filterDate: null,
      tableFields: [
        'business_id',
        'business_name',
        'business_address',
        'business_city',
        'business_state',
        'business_postal_code',
        'business_phone_number',
        'business_location',
        'inspection_date',
        'inspection_score',
        'inspection_description',
        'inspection_type',
        'violation_description',
        'violation_code',
      ],
      editableFields: {
        business_name: false,
        business_address: false,
        business_city: false,
        business_phone_number: false,
        business_postal_code: true,
        inspection_date: true,
        inspection_description: true,
        inspection_type: true,
      },
      fieldsDescr: {
        _id: 'Идетнификатор из базы',
        business_id: 'Идентификатор оргазинации',
        business_name: 'Наименование организации',
        business_address: 'Адрес организации',
        business_city: 'Город организации',
        business_state: 'Штат организации',
        business_postal_code: 'Почтовый индекс организации',
        business_phone_number: 'Телефон организации',
        business_location: 'Описание местоположения организации',
        inspection_date: 'Дата инспекции',
        inspection_score: 'Уровень оценки инспекции',
        inspection_description: 'Результат проверки',
        inspection_type: 'Тип проверки',
        violation_description: 'Описание нарушения',
        violation_code: 'Код нарушение',
      },
    };
  },
  methods: {
    toggleField(name) {
      this.editableFields[name] = !this.editableFields[name];
    },
    setDateFilter(date) {
      this.filterDate = date;
    },
    setSort(sortField) {
      if (this.sortOptions.sortBy !== sortField)
        this.sortOptions.sortBy = sortField;
      else this.sortOptions.sortBy = '';
    },
    sortedFunc(a, b) {
      const nameA = a[this.sortOptions.sortBy].toLowerCase();
      const nameB = b[this.sortOptions.sortBy].toLowerCase();
      if (nameA < nameB) return -1;
      if (nameA > nameB) return 1;
      return 0;
    },
  },
  computed: {
    datesCollection() {
      return Array.from(
        new Set(this.rowsWithMaches.map((business) => business.inspection_date))
      );
    },

    fieldHeadersForShow() {
      return this.tableFields.filter(
        (fieldName) =>
          !Object.prototype.hasOwnProperty.call(
            this.editableFields,
            fieldName
          ) || this.editableFields[fieldName]
      );
    },

    fieldsForFind() {
      return Object.keys(this.editableFields).filter(
        (key) => this.editableFields[key]
      );
    },

    rowsWithMaches() {
      let filteredDB = [];
      if (this.fieldsForFind.length)
        filteredDB = this.database.filter((business) =>
          this.fieldsForFind.some((key) =>
            business[key].toLowerCase().includes(this.searchQuery.toLowerCase())
          )
        );
      else filteredDB = this.database;
      return filteredDB;
    },

    rowsByDate() {
      let filteredDB = [];
      if (this.filterDate)
        filteredDB = this.rowsWithMaches.filter(
          (business) => business.inspection_date === this.filterDate
        );
      else filteredDB = this.rowsWithMaches;
      return filteredDB;
    },

    sortedDatabase() {
      const database = [...this.rowsByDate];
      if (this.sortOptions.sortBy) database.sort(this.sortedFunc);
      return database;
    },
  },
};
</script>

<style lang="scss">
.table {
  margin: 5px;
}
.table-settings-list {
  width: 100%;
  border: 1px solid #000;
  border-bottom: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
}
.table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid #000;
  border-top-width: 2px;
}
.table-title {
  display: inline-block;
  margin: 0 1rem;
}
.table-footer {
  text-align: right;
  font-size: 0.9rem;
  padding: 5px 1rem;
}

.search-input {
  padding: 5px;
}
.table-settings {
  min-width: 250px;
  display: inline-block;
  vertical-align: middle;
  border: 1px solid gray;
  border-radius: 0;
  height: 2em;
  margin: 5px 2px;
}
</style>
