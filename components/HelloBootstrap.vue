<template>
  <div>
    <h1>Student</h1>
    <Student @save="saveStudent" :itemStudent="this.student" />
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
      <b-button size="sm" @click="clickEdit" :disabled="disableEditButton">EDIT</b-button>
      <b-button size="sm" @click="clickDelete" :disabled="disableDeleteButton">DELETE</b-button>
    </p>
    <p>
      Selected Rows:<br>
      {{ selected }}
      {{this.selected.length}}
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
        student:{},
        selected: [],
        disableEditButton: true,
        disableDeleteButton:true,
      }
    },
    components:{
      Student,
    },
    watch:{
      selected(){
        if(this.selected.length == 1) this.disableEditButton = false;
        else this.disableEditButton = true;
        if(this.selected.length != 0) this.disableDeleteButton = false;
        else this.disableDeleteButton = true;
      }
    },
    methods: {
      saveStudent(itemStudent){
        console.log(itemStudent.id !='');
        if(itemStudent.id !=''){
          //Update the student
          let index = this.items.findIndex((c)=>c.id === itemStudent.id);
          this.items.splice(index,1,itemStudent);
        }else{
          //Add new Student
          itemStudent.id = this.findMaxID() + 1;
          this.items.push(itemStudent);
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
        this.$bvModal.show('modal-prevent-closing')
      },
      clickDelete(){
        for(let i = 0; i < this.selected.length; i++){
          this.deleteOneStudentById(this.selected[i].id);
        }
      },
      deleteOneStudentById(id){
        let index = this.items.findIndex((c)=>c.id === id);
        this.items.splice(index,1);
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
    }
  }
</script>