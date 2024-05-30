<template>
  <div>
    <b-button v-b-modal.modal-prevent-closing>Add</b-button>
    <b-modal
      id="modal-prevent-closing"
      ref="modal"
      title="Submit Your Name"
      @show="resetModal"
      @hidden="resetModal"
      @ok="handleOk"
    >
      <form ref="form" @submit.stop.prevent="handleSubmit">
        <!-- Name -->
        <b-form-group
          label="Name"
          label-for="name-input"
          invalid-feedback="Name is required"
          :state="nameState"
        >
          <b-form-input
            id="name-input"
            v-model="student.name"
            :state="nameState"
            required
          ></b-form-input>
        </b-form-group>
        <!-- Age -->
        <b-form-group
          label="Age"
          label-for="age-input"
          invalid-feedback="Age is required"
          :state="ageState"
        >
          <b-form-input
            id="age-input"
            v-model="student.age"
            :state="ageState"
            required
          ></b-form-input>
        </b-form-group>
        <!-- Phone -->
        <b-form-group
          label="Phone"
          label-for="phone-input"
          invalid-feedback="Phone is required"
          :state="phoneState"
        >
          <b-form-input
            id="phone-input"
            v-model="student.phone"
            :state="phoneState"
            required
          ></b-form-input>
        </b-form-group>
        <!-- Address -->
        <b-form-group
          label="Adress"
          label-for="addess-input"
          invalid-feedback="Address is required"
          :state="addressState"
        >
          <b-form-input
            id="address-input"
            v-model="student.address"
            :state="addressState"
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
          age: '',
          phone: '',
          address: ''
        },
        nameState: null,
        ageState: null,
        phoneState: null,
        addressState: null
      }
    },
    methods: {
      checkFormValidity() {
        const valid = this.$refs.form.checkValidity()
        this.nameState = valid
        this.ageState = valid
        this.phoneState = valid
        this.addressState = valid
        return valid
      },
      resetModal() {
        this.name = ''
        this.nameState = null
      },
      handleOk(bvModalEvent) {
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
        // Hide the modal manually
        this.$nextTick(() => {
          this.$bvModal.hide('modal-prevent-closing')
        })
      }
    }
  }
</script>