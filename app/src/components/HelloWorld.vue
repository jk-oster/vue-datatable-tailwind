<!-- /* eslint-disable */ -->
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
  <input type="text" v-model="searchValue" />
  <Vue3EasyDataTable
    :headers="headers"
    :items="filteredItems"
    table-class-name="custom-table"
    :body-row-class-name="bodyRowClassNameFunction"
    :body-item-class-name="bodyItemClassNameFunction"
    :header-item-class-name="headerItemClassNameFunction"
    :filter-options="filterOptions"
    alternating
    buttons-pagination
    :rows-items="[5, 10, 25, 50]"
    :rows-per-page="25"
    theme-color="red"
    :search-field="searchField"
    :search-value="searchValue"
  >
    <template #header-calories="header">
      <div class="filter-column">
        <img
          src="../assets/filter.png"
          class="filter-icon"
          @click.stop="showCalorieFilter = !showCalorieFilter"
        />
        {{ header.text }}
        <div class="filter-menu" v-if="showCalorieFilter">
          <label for="caloriesCriteria">Calories ></label>
          <input id="caloriesCriteria" v-model="caloriesCriteria" />
        </div>
      </div>
    </template>
  </Vue3EasyDataTable>
</template>

<script>
import Vue3EasyDataTable from "vue3-easy-data-table";
import "vue3-easy-data-table/dist/style.css";

export default {
  name: "HelloWorld",
  components: {
    Vue3EasyDataTable,
  },
  props: {
    items: Array,
  },
  data() {
    return {
      searchField: "name",
      searchValue: "",
      filter: true,
      // itemsSelected: this.items[0],
      caloriesCriteria: 0,
      showCalorieFilter: false,
      headers: [
        {
          text: "",
          value: "indicator",
          sortable: true,
        },
        {
          text: "Dessert (100g serving)",
          sortable: true,
          value: "name",
        },
        { text: "Calories", value: "calories", sortable: true },
        { text: "Fat (g)", value: "fat", sortable: true },
        { text: "Carbs (g)", value: "carbs", sortable: true },
        { text: "Protein (g)", value: "protein", sortable: true },
        { text: "Iron (%)", value: "iron", sortable: true },
        { text: "Difference", value: "difference", sortable: true },
      ],
    };
  },
  mounted() {
    document.querySelectorAll('.indicator').forEach(elem => {
      const indicator = elem.firstElementChild;
      if(indicator.textContent == "attention") elem.classList.add('indicator-yellow');
    })
  },
  updated() {
    console.log('updated');
    document.querySelectorAll('.indicator').forEach(elem => {
      const indicator = elem.firstElementChild;
      if(indicator.textContent == "attention") elem.classList.add('indicator-yellow');
    })
  },
  computed: {
    filteredItems() {
      return (
        this.items
          // .filter((item) => item.calories > 300)
          .map((item) => {
            item.difference = item.calories - item.carbs;
            item.indicator = item.difference > 100 ? "attention" : "";
            return item;
          })
      );
    },
    filterOptions() {
      const filterOptions = [];
      if (this.caloriesCriteria !== 0) {
        filterOptions.push({
          field: "calories",
          comparison: ">",
          criteria: this.caloriesCriteria,
        });
      }
      return filterOptions;
    },
    // eslint-disable-next-line
    bodyRowClassNameFunction(item, index) {
      console.log(item);
      console.log(index);
      if (item.calories > 300) return "indicator indicator-yellow";
      else if (item.calories > 450)return "indicator indicator-red";
      return "indicator";
    },
    // eslint-disable-next-line
    bodyItemClassNameFunction(column, index) {
      console.log(column);
      if (column === "calories") return "bg-red-600";
      return "bg-blue-600";
    },
    // eslint-disable-next-line
    headerItemClassNameFunction(header, index) {
      console.log(header);
      if (header.value === "calories") return "bg-gray-600";
      return "bg-green-600";
    },
  },
};
</script>

<style>
.indicator td:first-child {
  /* Hide the text. */
  font-size: 0 !important;
}
.indicator td:first-child::after {
  content: "";
  display: inline-block;
  width: 15px;
  height: 15px;
  -moz-border-radius: 7.5px;
  -webkit-border-radius: 7.5px;
  border-radius: 7.5px;
  background-color: #69b6d5;
}
.indicator-red td:first-child::after {
  background-color: red;
}
.indicator-yellow td:first-child::after {
  background-color: yellow;
}

.custom-table {
  --easy-table-border: 1px solid #445269;
  --easy-table-row-border: 1px solid #445269;

  --easy-table-header-font-size: 14px;
  --easy-table-header-height: 50px;
  --easy-table-header-font-color: #c1cad4;
  --easy-table-header-background-color: #2d3a4f;

  --easy-table-header-item-padding: 10px 15px;

  --easy-table-body-even-row-font-color: #fff;
  --easy-table-body-even-row-background-color: #4c5d7a;

  --easy-table-body-row-font-color: #c0c7d2;
  --easy-table-body-row-background-color: #2d3a4f;
  --easy-table-body-row-height: 50px;
  --easy-table-body-row-font-size: 14px;

  --easy-table-body-row-hover-font-color: #2d3a4f;
  --easy-table-body-row-hover-background-color: #eee;

  --easy-table-body-item-padding: 10px 15px;

  --easy-table-footer-background-color: #2d3a4f;
  --easy-table-footer-font-color: #c0c7d2;
  --easy-table-footer-font-size: 14px;
  --easy-table-footer-padding: 0px 10px;
  --easy-table-footer-height: 50px;

  --easy-table-rows-per-page-selector-width: 70px;
  --easy-table-rows-per-page-selector-option-padding: 10px;

  --easy-table-scrollbar-track-color: #2d3a4f;
  --easy-table-scrollbar-color: #2d3a4f;
  --easy-table-scrollbar-thumb-color: #4c5d7a;
  --easy-table-scrollbar-corner-color: #2d3a4f;

  --easy-table-loading-mask-background-color: #2d3a4f;
}

.filter-column {
  display: flex;
  align-items: center;
  justify-items: center;
  position: relative;
}

.filter-icon {
  cursor: pointer;
  display: inline-block;
  width: 15px !important;
  height: 15px !important;
  margin-right: 4px;
}

.filter-menu {
  padding: 5px;
  z-index: 1;
  position: absolute;
  top: 30px;
  width: 200px;
  background-color: #2d3a4f;
  border: 1px solid #e0e0e0;
}
</style>
