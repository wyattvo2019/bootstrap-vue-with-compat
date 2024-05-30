<template>
  <div>
    <h1>Student Manage</h1>
    <Student @save="clickSave" :itemStudent="this.student" />
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
      <b-button size="sm" @click="clickEdit" :disabled="statusEditButton">Edit</b-button>
      <b-button size="sm" @click="clickDelete" :disabled="statusDeleteButton">Delete</b-button>

    </p>
    <p>
      Selected Rows:<br>
      {{ selected }}
    </p>
  </div>
</template>

<script>
import Student from "./StudentComponent.vue";
  export default {
    components:{
      Student
    },
    data() {
      return {
        fields: ['selected', 'id', 'name', 'age', 'phone', 'address'],
        items: [
          { id: 1, name: 'Le Van A', age: 25, phone: 111111, address: "HN" },
          { id: 2, name: 'Le Van B', age: 26, phone: 222222, address: "ND" },
          { id: 3, name: 'Le Van C', age: 27, phone: 333333, address: "VP" },
          { id: 4, name: 'Le Van D', age: 29, phone: 828282, address: "TB" },
        ],
        student:{},
        selected: [],
        statusEditButton: false,
        statusDeleteButton: false,
      }
    },
    watch:{
      selected(){
        console.log(this.selected.length==1);
        if(this.selected.length === 1){  this.statusEditButton = false;}
        else this.statusEditButton = true;
        if(this.selected.length > 0){this.statusDeleteButton = false;} 
        else this.statusDeleteButton = true;
      }
    },
    methods: {
      clickSave(itemSave){
        if(itemSave.id){
          let index = this.items.findIndex((c)=>c.id===itemSave.id);
          this.items.splice(index,1,itemSave);
        }else{
          itemSave.id = this.findMaxID() + 1;
          this.items.push(itemSave);
        }
      },
      findMaxID(){
        let maxID = this.items[0].id;
        for(let i = 1; i < this.items.length; i++){
          if(this.items[i].id > maxID) maxID = this.items[i].id;
        }
        return maxID
      },
      clickEdit(){
        this.student = this.selected[0];
        console.log(this.student);
        this.$bvModal.show('modal-prevent-closing');
      },
      clickDelete(){
        for(let i = 0; i < this.selected.length; i++){
          this.deleteOneStudentById(this.selected[i].id);
        }
      },
      deleteOneStudentById(id){
        for(let i = 0; i < this.items.length; i++){
          if(this.items[i].id === id) this.items.splice(i,1);
        }
      },
      onRowSelected(items) {
        this.selected = items
      },
      selectAllRows() {
        this.$refs.selectableTable.selectAllRows()
      },
      clearSelected() {
        this.$refs.selectableTable.clearSelected()
      },
      selectThirdRow() {
        // Rows are indexed from 0, so the third row is index 2
        this.$refs.selectableTable.selectRow(2)
      },
      unselectThirdRow() {
        // Rows are indexed from 0, so the third row is index 2
        this.$refs.selectableTable.unselectRow(2)
      }
    }
  }
</script>