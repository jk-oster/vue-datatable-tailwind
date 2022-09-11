
<template>
  <!-- <table :id="'table-comp' + tableId" data-turbolinks="false">
    <thead>
      <tr>
        <th v-for="header in headers" :key="header.text">{{header.text}}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in items" :key="item.name">
        <td v-for="key in Object.keys(item)" :key="item[key]">{{item[key]}}</td>
      </tr>
    </tbody>
  </table> -->
  <span>Search by:</span>
  <select v-model="searchField">
    <option value="name">Dessert</option>
    <option value="calories">Calories</option>
  </select>

  <br />

  <span>Search: </span>
  <input type="text" v-model="searchValue">
  <Vue3EasyDataTable 
    :headers="headers" 
    :items="filteredItems" 
    alternating 
    buttons-pagaination
    :rows-items="[5,10,25,50]"
    :rows-per-page="25"
    theme-color="#42b883"
    :search-field="searchField"
    :search-value="searchValue"
    >
    <template #header-name="header">
      <div class="filter-column">
        <img src="../images/eglass-filter.png" class="filter-icon" @click.stop="showNameFilter=!showNameFilter">
        {{ header.text }}
        <div class="filter-menu" v-if="showNameFilter">
          <input v-model="nameCriteria" />
        </div>
      </div>
    </template>

  </Vue3EasyDataTable>

</template>

<script>
import Vue3EasyDataTable from 'vue3-easy-data-table';
import 'vue3-easy-data-table/dist/style.css';

export default {
  name: 'HelloWorld',
  components: {
    Vue3EasyDataTable
  },
  props: {
    tableId: String,
    headers: Array,
    items: Array
  },
  data() {
    return {
      datatable: null,
      searchField: "name",
      searchValue: "",
      filter: true,
    }
  },
  computed: {
    filteredItems() {
      return this.items.filter(item => item.calories > 300)
    }
  }
}
</script>
