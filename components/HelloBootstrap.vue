<template>
  <div>
    <h1>Student</h1>
    <Student />
    <b-table
      :items="items"
      :fields="fields"
      responsive="sm"
      ref="selectableTable"
      selectable
      @row-selected="onRowSelected"
    >
      <!-- Example scoped slot for select state illustrative purposes -->
      <template #cell(selected)="{ rowSelected }">
        <template v-if="rowSelected">
          <span aria-hidden="true">&check;</span>
          <span class="sr-only">Selected</span>
        </template>
        <template v-else>
          <span aria-hidden="true">&nbsp;</span>
          <span class="sr-only">Not selected</span>
        </template>
      </template>
    </b-table>
    <p>
      <b-button size="sm" @click="selectAllRows">Select all</b-button>
      <b-button size="sm" @click="clearSelected">Clear selected</b-button>
      <b-button size="sm" @click="clickEdit">EDIT</b-button>
      <b-button size="sm" @click="clickDelete">DELETE</b-button>
    </p>
    <p>
      Selected Rows:<br>
      {{ selected }}
    </p>
  </div>
</template>

<script>
import Student from './StudentComponent.vue';
  export default {
    data() {
      return {
        fields: ['selected', 'id', 'name', 'age', 'phone', 'address'],
        items: [
          { id: 4, name: "Le Van A", age: 28, phone: 84159358429, address: "Ha Noi" },
          { id: 3, name: "Le Van B", age: 29, phone: 84789654137, address: "Thai Binh" },
          { id: 1, name: "Le Van C", age: 26, phone: 84745896315, address: "Vinh Phuc" },
          { id: 2, name: "Le Van D", age: 27, phone: 84785493144, address: "Ha Nam" },
        ],
        selected: []
      }
    },
    components:{
      Student,
    },
    methods: {
      onRowSelected(items) {
        this.selected = items
      },
      selectAllRows() {
        this.$refs.selectableTable.selectAllRows()
      },
      clearSelected() {
        this.$refs.selectableTable.clearSelected()
      },
    }
  }
</script>