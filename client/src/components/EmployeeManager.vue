<template>
    <div class="employee-manager">
      <div class="header">
        <h2>Nhân viên</h2>
        <button class="btn-add">Thêm mới nhân viên</button>
      </div>
  
      <div class="search-container">
        <input type="text" placeholder="Tìm kiếm theo email" v-model="searchTerm" />
        <button @click="searchEmployee">🔍</button>
      </div>
  
      <table class="employee-table">
        <thead class="nav">
          <tr>
            <th>STT</th>
            <th>Họ và tên</th>
            <th>Ngày sinh</th>
            <th>Email</th>
            <th>Địa chỉ</th>
            <th>Trạng thái</th>
            <th>Chức năng</th>
          </tr>
        </thead>
        <tbody>
          <tr v-if="employees.length === 0">
            <td colspan="7">Không có dữ liệu</td>
          </tr>
          <tr v-else v-for="(employee, index) in filteredEmployees" :key="employee.id">
            <td>{{ index + 1 }}</td>
            <td>{{ employee.name }}</td>
            <td>{{ employee.birthdate }}</td>
            <td>{{ employee.email }}</td>
            <td>{{ employee.address }}</td>
            <td>
              <span :class="employee.statusClass">{{ employee.status }}</span>
            </td>
            <td>
              <button class="btn-green" @click="blockEmployee(index)">Chặn</button>
              <button class="btn-yellow" @click="editEmployee(index)">Sửa</button>
              <button class="btn-red" @click="deleteEmployee(index)">Xóa</button>
            </td>
          </tr>
        </tbody>
      </table>
  
      <div class="pagination">
        <label>Hiển thị</label>
        <select v-model="recordsPerPage">
            <option v-for="(option, index) in paginationOptions" :key="index" :value="option">{{ option }}</option>
        </select>
        <span>bản ghi trên trang</span>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed, onMounted } from 'vue'
  import axios from 'axios'
  
  const employees = ref([])
  const searchTerm = ref('')
  const recordsPerPage = ref(10)
  const paginationOptions = [5, 10, 20]
  
  const fetchEmployees = async () => {
    try {
      const response = await axios.get('http://localhost:1007/employees')
      employees.value = response.data
    } catch (error) {
      console.error('Lỗi khi lấy dữ liệu nhân viên:', error)
    }
  }
  
  const filteredEmployees = computed(() => {
    return employees.value.filter(employee =>
      employee.email.toLowerCase().includes(searchTerm.value.toLowerCase())
    )
  })
  
  onMounted(fetchEmployees)
  
  const searchEmployee = () => {
    console.log('Tìm kiếm với từ khóa:', searchTerm.value)
  }
  
  const blockEmployee = (index) => {
    console.log('Chặn nhân viên:', employees.value[index])
  }
  
  const editEmployee = (index) => {
    console.log('Sửa nhân viên:', employees.value[index])
  }
  
  const deleteEmployee = (index) => {
    employees.value.splice(index, 1)
  }
  </script>
  
  <style scoped>
  .nav{
    color:black;
  }
  .employee-manager {
    padding: 20px;
  }
  
  .header {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
  }
  
  .btn-add {
    background-color: #007bff;
    color: white;
    padding: 8px 12px;
    border: none;
    cursor: pointer;
  }
  
  .search-container {
    display: flex;
    justify-content: flex-end;
    margin-bottom: 10px;
  }
  
  .employee-table {
    width: 100%;
    border-collapse: collapse;
  }
  
  .employee-table th, .employee-table td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: center;
  }
  
  .employee-table th {
    background-color: #f2f2f2;
  }
  
  .btn-green {
    background-color: #28a745;
    color: white;
    padding: 5px 10px;
    border: none;
    margin-right: 5px;
  }
  
  .btn-yellow {
    background-color: #ffc107;
    color: white;
    padding: 5px 10px;
    border: none;
    margin-right: 5px;
  }
  
  .btn-red {
    background-color: #dc3545;
    color: white;
    padding: 5px 10px;
    border: none;
  }
  
  .pagination {
    margin-top: 20px;
    display: flex;
    align-items: center;
  }
  </style>
  