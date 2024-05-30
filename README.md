# bootstrap-vue-with-compat

## 1. Get UI for HelloBootstrap from:
```
https://bootstrap-vue.org/docs/components/table#row-select-support
```
## 2. Remove some useless code
```
```
## 3. Restructure the student data
```
        fields: ['selected', 'id', 'name', 'age', 'phone', 'address'],
        items: [
          { id: 4, name: "Le Van A", age: 28, phone: 84159358429, address: "Ha Noi" },
          { id: 3, name: "Le Van B", age: 29, phone: 84789654137, address: "Thai Binh" },
          { id: 1, name: "Le Van C", age: 26, phone: 84745896315, address: "Vinh Phuc" },
          { id: 2, name: "Le Van D", age: 27, phone: 84785493144, address: "Ha Nam" },
        ],
```
## 4. Get form for student from:
```
https://bootstrap-vue.org/docs/components/modal#prevent-closing
```
## 5. Update the form Student:
```
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
..........................
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
..........................
      checkFormValidity() {
        const valid = this.$refs.form.checkValidity()
        this.nameState = valid
        this.ageState = valid
        this.phoneState = valid
        this.addressState = valid
        return valid
      },
...........................
```