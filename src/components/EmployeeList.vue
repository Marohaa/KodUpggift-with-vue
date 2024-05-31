<template>
    <div class="title">
        <h1>Anställda</h1>
    </div>
    <div class="employee-list">

      <div v-for="employee in employees" :key="employee.id" class="employee">
        <img :src="employee.avatar" alt="Profile Picture" class="profile-picture">
        <div class="employee-info">
          <h3>{{ employee.first_name }} {{ employee.last_name }}</h3>
          <a :href="'mailto:' + employee.email">{{ employee.email }}</a>
        </div>
      </div>
      <div class="button-container" v-if="!allEmployeesLoaded">
        <button @click="loadNextPage">Se alla anställda</button>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        employees: [],
        currentPage: 1,
        totalPages: 1,
        allEmployeesLoaded: false
      };
    },
    created() {
      this.fetchEmployees(this.currentPage);
    },
    methods: {
      async fetchEmployees(page) {
        const response = await axios.get(`https://reqres.in/api/users?page=${page}`);
        this.employees = this.employees.concat(response.data.data);
        this.totalPages = response.data.total_pages;
        if (page >= this.totalPages) {
          this.allEmployeesLoaded = true;
        }
      },
      async loadNextPage() {
        this.currentPage++;
        if (this.currentPage <= this.totalPages) {
          await this.fetchEmployees(this.currentPage);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .title{
    display: flex;
    justify-content: center;
    margin-top: 20px;
  }
  .employee-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  .employee {
    border: 1px solid #ddd;
    border-radius: 10px;
    margin: 10px;
    padding: 20px;
    text-align: center;
    width: 200px;
  }
  
  .profile-picture {
    border-radius: 50%;
    width: 100px;
    height: 100px;
  }
  
  .employee-info {
    margin-top: 10px;
  }
  .employee-info a{
    color: #000000;
  }
  .button-container {
    width: 100%;
    display: flex;
    justify-content: center;
    margin: 20px;
  }
  
  button {
    padding: 10px 20px;
    background-color: #003d7f;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #000000;
  }
  
  @media (max-width: 600px) {
    .employee {
      width: 100%;
      margin: 5px 0;
    }
  }
  </style>
  
  
  