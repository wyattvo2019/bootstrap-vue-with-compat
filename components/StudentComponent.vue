<template>
  <div>
    <b-button v-b-modal.modal-prevent-closing>Add</b-button>
    <!-- <div class="mt-3">
      Submitted Names:
      <div v-if="submittedNames.length === 0">--</div>
      <ul v-else class="mb-0 pl-3">
        <li v-for="name in submittedNames">{{ name }}</li>
      </ul>
    </div> -->

    <b-modal
      id="modal-prevent-closing"
      ref="modal"
      title="Submit Your Name"
      @show="resetModal"
      @hidden="resetModal"
      @ok="handleOk"
    >
      <form ref="form" @submit.stop.prevent="handleSubmit">
        <b-form-group
          label="Name"
          label-for="name-input"
          invalid-feedback="Name is required"
        >
          <b-form-input
            id="name-input"
            v-model="student.name"
            required
          ></b-form-input>
        </b-form-group>
        <b-form-group
        label="Age"
        label-for="age-input"
        invalid-feedback="Age is required"
      >
        <b-form-input
          id="age-input"
          v-model="student.age"
          required
        ></b-form-input>
      </b-form-group>
      <b-form-group
      label="Phone"
      label-for="phone-input"
      invalid-feedback="Phone is required"
    >
      <b-form-input
        id="phone-input"
        v-model="student.phone"
        required
      ></b-form-input>
    </b-form-group>
    <b-form-group
    label="Address"
    label-for="address-input"
    invalid-feedback="Address is required"
  >
    <b-form-input
      id="address-input"
      v-model="student.address"
      required
    ></b-form-input>
  </b-form-group>
      </form>
    </b-modal>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        student: {
          id: '',
          name: '',
          age:'',
          phone:'',
          address:''
        },
        name: '',
        nameState: null,
        submittedNames: []
      }
    },
    props:{
      itemStudent:{
        type: Object,
        default: null,
      }
    },
    watch:{
      itemStudent(){
        if(this.itemStudent) this.student = Object.assign({}, this.itemStudent);
        else this.clearForm();
      }
    },
    methods: {
      clearForm(){
        this.student= {
          id: '',
          name: '',
          age:'',
          phone:'',
          address:''
        };
      },
      checkFormValidity() {
        const valid = this.$refs.form.checkValidity()
        this.nameState = valid
        return valid
      },
      resetModal() {
        this.name = ''
        this.nameState = null
      },
      handleOk(bvModalEvent) {
        this.$emit('save', this.student);
        // Prevent modal from closing
        bvModalEvent.preventDefault()
        // Trigger submit handler
        this.handleSubmit()
      },
      handleSubmit() {
        // Exit when the form isn't valid
        if (!this.checkFormValidity()) {
          return
        }
        // Push the name to submitted names
        // this.submittedNames.push(this.name)
        // Hide the modal manually
        this.$nextTick(() => {
          this.$bvModal.hide('modal-prevent-closing')
        })
      }
    }
  }
</script>