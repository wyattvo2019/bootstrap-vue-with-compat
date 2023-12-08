
<template>
  <StudentComponent :editStudent="editStudent" @save="saveStudent"/>
    <div>
  
      <b-table
        :items="items"
        :fields="fields"
        :select-mode="selectMode"
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
        <b-button size="sm" @click="clickEdit" :disabled="disableEditButton">Edit</b-button>
        <b-button size="sm" @click="clickDelete" :disabled="disableDeleteButton">Delete</b-button>
      </p>
      <p>
        Selected Rows:<br>
        {{ selected }}
      </p>
    </div>
</template>
  
<script>
import StudentComponent from './StudentComponent.vue'
export default {
  components: {
    StudentComponent,
  },
  data(){
    return {
      editStudent:{},
      disableEditButton: true,
      disableDeleteButton: true,
      // modes: ['multi', 'single', 'range'],
      fields: ['selected', 'id','name', 'age', 'phone', 'address'],
      items: [
        { id: 1,name: "Nguyen Huu Nghia", age: 40, phone: 8888, address: 'TTH' },
        { id: 2,name: "Hoang Duy Khanh", age: 40, phone: 9999, address: 'PY' },
        { id:3,name: "Tran Duy Hung", age: 40, phone: 7777, address: 'DN' },
        { id:4, name: "Pham Thuc Phuoc", age: 40, phone: 6666, address: 'QN' }
      ],
      selectMode: 'multi',
      selected: []
    }
  },
  watch:{
    selected(){
      if(this.selected.length == 1){
        this.disableEditButton = false;
      } 
      else this.disableEditButton = true;
      if(this.selected.length > 0){
        this.disableDeleteButton = false; 
      } 
      else this.disableDeleteButton = true;
    },
  },
  methods: {
    clickDelete(){
      if(this.selected.length >0){
        //delete
        for(let i=0; i < this.selected.length; i++){
          let index = this.items.findIndex((c)=> c.id = this.selected[0].id);
          this.items.splice(index,1);
        }
      }else{
        return;
      }
    },
    saveStudent(studentData){
      console.log(studentData);
      //check is add or edit
      if(studentData.id){
        //edit
        let index = this.items.findIndex((c)=>c.id === studentData.id)
        this.items.splice(index,1,studentData);
      }else{
        //add new
        let maxId = 0;
        for(let i=0; i< this.items.length;i++){
          if(maxId < this.items[i].id) maxId=this.items[i].id;
        }
        studentData.id = maxId +1;
        this.items.push(studentData);
      }
    },
    clickEdit(){
      this.editStudent = this.selected[0];
      console.log(this.editStudent);
      this.showModal();
    },
    showModal() {
      // this.$bvModal.msgBoxOk('Modal from @vue/compat');
      this.$bvModal.show('modal-prevent-closing')
    },
    onRowSelected(items) {
      this.selected = items
      console.log(this.selected.length)
      console.log(items)
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

