<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>

    <employee-form @add:employee="addEmployee" />
    <employee-table :employees="employees"
    @delete:employee="deleteEmployee"
    @edit:employee="editEmployee"/>
  </div>
</template>

<script>
import EmployeeTable from '@/components/EmployeeTable.vue'
import EmployeeForm from '@/components/EmployeeForm.vue'

export default {
  name: 'app',
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data() {
    return{
      employees:[
        {
          id: 1,
          name: 'Muhamad Sirojudin',
          email: 'sirojudin145@incuabah.id'
        },
        {
          id: 2,
          name: 'Muhamad Damanhuri',
          email: 'dm465@incuabah.id'
        },
        {
          id: 3,
          name: 'Raden Ainul Yaqin',
          email: 'raden@incuabah.id'
        },
        {
          id: 4,
          name: 'Siti Rodiyah',
          email: 'rodiyah12@incuabah.id'
        },
        {
          id: 5,
          name: 'H. Sukria',
          email: 'sukriaabdus@incuabah.id'
        }
      ]
    }
  },
  methods: {

    addEmployee(employee) {
      const lastId =
        this.employees.length > 0
          ? this.employees[this.employees.length - 1].id
          : 0;
      const id = lastId + 1;
      const newEmployee = { ...employee, id };
      
      this.employees = [...this.employees, newEmployee]
    },

    deleteEmployee(id){
      this.employees = this.employee.filter(
        employee => employee.id !== id
      )
    },

    editEmployee(id, updatedEmployee){
      this.employees = this.employees.map(employee =>
        employee.id === id ? updatedEmployee : employee
      )
    },

    //to get data from API
    async getEmployees() {
      try{
        const response = await fetch('https://jsonplaceholder.typicode.com/users')
        const data = await response.json()
        this.employees = data
      }catch(error){
        console.error(error)
      }
    },

    //adding data to API
    async addEmployee(employee){
      try{
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: {'Content-type': 'application/json; charset=UTF-8'}
        })
        const data = await response.json()
        this.employees = [...this.employees, data]
      }catch(error){
        console.error(error)
      }
    },

    //update an existing resource
    async editEmployee(id, updatedEmployee){
      try{
        const response = await fetch('https://jsonplaceholder.typicode.com/users/${id}', {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: {'Content-type': 'application/json; charset=UTF-8'}
        })

        const data = await response.json()
        this.employees = this.employees.map(employee =>
          (employee.id === id ? data : employee)
        )
      }catch(error){
        console.error(error)
      }
    },

    //delete an existing resource
    async deleteEmployee(id){
      try{
        await fetch('https://jsonplaceholder.typicode.com/users/${id}', {
          method: 'DELETE'
        })
        this.employees = this.employees.filter(employee => employee.id !== id)
      }catch(error){
        console.error(error)
      }
    }
  },

  //on mounted all DOM already
  mounted(){
    this.getEmployees()
  },
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
  }


  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 680px;
  }
</style>
