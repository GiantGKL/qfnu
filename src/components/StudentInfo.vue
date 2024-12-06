<template>
  <div>
    <div v-if="students.length">
      <div v-for="student in students":key="student.id">
        <div>姓名: {{ student.name }}</div>
        <div>学号: {{ student.studentID }}</div>
      </div>
    </div>
    <p v-else>加载失败</p>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      students: []
    };
  },
  created() {
    this.fetchStudents();
  },
  methods: {
    async fetchStudents() {
      try {
        const response = await axios.get('http://localhost:3000/api/student');
        this.students = response.data;
      } catch (error) {
        console.error('获取学生数据失败:', error);
      }
    }
  }
};
</script>

<style scoped>
</style>
