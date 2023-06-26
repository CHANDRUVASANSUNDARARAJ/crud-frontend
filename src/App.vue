<template>
  <div>
    <h1>Employee Registration</h1>
    <form id="employee-form" @submit="addEmployee">
      <label for="name">Name:</label>
      <input type="text" width="50px" id="name" name="name" v-model="newEmployee.name" required>

      <label for="email">Email:</label>
      <input type="email" id="email" name="email" v-model="newEmployee.email" required>

      <label for="department">Department:</label>
      <input type="text" id="department" name="department" v-model="newEmployee.department" required>

      <label for="salary">Salary:</label>
      <input type="number" id="salary" name="salary" v-model="newEmployee.salary" required>

      <input v-if="!isUpdating" type="submit" value="Submit">
      <input v-if="isUpdating" type="submit" value="Update" @click.prevent="updd">
    </form>

    <div id="employee-list">
      <h2 align="center">Employee Details</h2>
      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Email</th>
            <th>Department</th>
            <th>Salary</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(employee, index) in employees" :key="index">
            <td>{{ employee.name }}</td>
            <td>{{ employee.email }}</td>
            <td>{{ employee.department }}</td>
            <td>{{ employee.salary }}</td>
            <td>
              <button @click="editEmployee(index)">Edit</button>
              &nbsp;&nbsp;&nbsp;&nbsp;
              <button @click="deleteEmployee(employee.id)">Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios"

export default {
  name: 'EmployeeRegistration',
  data() {
    return {
      employees: [],
      isUpdating:false,
      newEmployee: {
        name: '',
        email: '',
        department: '',
        salary: '',
      }
    }
  },
  methods: {
    addEmployee(e) {
      e.preventDefault()
      axios.post("http://localhost:8000/data", { name: this.newEmployee.name, department: this.newEmployee.department, salary: this.newEmployee.salary, email: this.newEmployee.email })
        .then(res => {
          console.log(res);
          this.getData()
          this.clearForm()
        })

    },
    editEmployee(index) {
      this.isUpdating = true
      this.newEmployee = { ...this.employees[index] };
      // this.employees.splice(index, 1);
    },
    updd(){
      axios.put("http://localhost:8000/"+this.newEmployee.id,this.newEmployee)
      .then(()=>{
this.getData()
this.clearForm()
this.isUpdating = false
      })
    },
    deleteEmployee(index) {

      axios.delete("http://localhost:8000/" + index)
      .then(()=>{
        this.getData()

      })
    },
    clearForm() {
      this.newEmployee = {
        name: '',
        email: '',
        department: '',
        salary: ''
      };
    },
    getData() {
      axios("http://localhost:8000/data")
        .then(res => {
          this.employees = res.data
          console.log(res.data);
        })
    }
  },
  mounted() {
    this.getData()
  }

}
</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 20px;
}

h1 {
  text-align: center;
}

form {
  max-width: 400px;
  margin: 0 auto;
}

label,
input {
  display: block;
  margin-bottom: 5px;
  background-color: beige;
  background-size: contain;
}

input[type="submit"] {
  background-color: #4CAF50;
  color: white;
  padding: 10px 15px;
  border: none;
  cursor: pointer;
}

#employee-list {
  margin-top: 20px;
}

#employee-list table {
  width: 100%;
  border-collapse: collapse;
}

#employee-list th,
#employee-list td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

button {
  cursor: pointer;
}

tr:hover {
  background-color: blue;
  font-weight: bold;
  color: blanchedalmond;
}

h2 {
  background-position: center;
  background-color: #4CAF50;
}

span {
  background-color: chartreuse;
  color: aliceblue;
}
</style>
