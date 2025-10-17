<script setup lang="ts">
import { ref, onMounted } from 'vue'
import StudentService from '@/services/StudentService'

interface Student {
  name: string
  surname: string
  gpa: number
}

const students = ref<Student[] | null>(null)

onMounted(() => {
  StudentService.getStudents()
    .then((response) => {
      students.value = response.data
    })
    .catch((error) => {
      console.error('Error fetching students:', error)
    })
})
</script>

<template>
  <div class="students-container">
    <h1>Student List</h1>
    <div v-if="students" class="student-cards">
      <div v-for="(student, index) in students" :key="index" class="student-card">
        <h3>{{ student.name }} {{ student.surname }}</h3>
        <p>GPA: {{ student.gpa }}</p>
      </div>
    </div>
    <div v-else class="loading">
      Loading students...
    </div>
  </div>
</template>

<style scoped>
.students-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.student-cards {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
  margin-top: 20px;
}

.student-card {
  background-color: #f8f9fa;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s, box-shadow 0.2s;
}

.student-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}

.student-card h3 {
  margin: 0 0 10px 0;
  color: #333;
}

.student-card p {
  margin: 0;
  color: #666;
  font-size: 1.1em;
}

.loading {
  text-align: center;
  padding: 40px;
  color: #666;
}
</style>