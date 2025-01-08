
<template>
  <div class="min-h-screen bg-gray-50 text-gray-900
  ">
    <header class="bg-blue-500 text-white py-4">
      <h1 class="text-center text-3xl font-bold">Student Performance Tracker</h1>
    </header>

    <main class="container mx-auto py-8">
      <section class="mb-8">
        <h2 class="text-xl font-semibold mb-4">Add Student Record</h2>
        <form @submit.prevent="addStudent"  class="flex flex-col gap-4 bg-white p-6 rounded-lg shadow-md  ">
          <input
            type="text"
            v-model="newStudent.name"
            placeholder="Student Name"
            class="border rounded px-3 py-2"
            required
          />
          <input
            type="text"
            v-model="newStudent.grade"
            placeholder="Grade"
            class="border rounded px-3 py-2"
            required
          />
          <input
            type="number"
            v-model="newStudent.score"
            placeholder="Performance Score"
            class="border rounded px-3 py-2"
            required
          />
          <button
            type="submit"
            class="bg-blue-500 text-white py-2 rounded hover:bg-blue-600 transition"
          >
            Add Student
          </button>
        </form>
      </section>

      <section class="mb-8">
        <h2 class="text-xl font-semibold mb-4">Student Records</h2>
        <input
          type="text"
          v-model="searchQuery"
          placeholder="Search by name"
          class="border rounded px-3 py-2 w-full mb-4"
        />
        <table class="w-full border-collapse bg-white rounded-lg shadow-md">
          <thead>
            <tr class="bg-gray-200">
              <th class="border px-4 py-2">Name</th>
              <th class="border px-4 py-2">Grade</th>
              <th class="border px-4 py-2">Score</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="student in filteredStudents" :key="student.name">
              <td class="border px-4 py-2">{{ student.name }}</td>
              <td class="border px-4 py-2">{{ student.grade }}</td>
              <td class="border px-4 py-2">{{ student.score }}</td>
            </tr>
          </tbody>
        </table>
      </section>

      <section>
        <h2 class="text-xl font-semibold mb-4">Average Performance by Grade</h2>
        <BarChart :data="chartData" />
      </section>
    </main>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import BarChart from './components/BarChart.vue';

export default {
  components: { BarChart },
  setup() {
    const students = ref([]);
    const newStudent = ref({ name: '', grade: '', score: '' });
    const searchQuery = ref('');

    const addStudent = () => {
      if (newStudent.value.name && newStudent.value.grade && newStudent.value.score) {
        students.value.push({ ...newStudent.value });
        newStudent.value = { name: '', grade: '', score: '' };
      }
    };

    const filteredStudents = computed(() => {
      return students.value.filter((student) =>
        student.name.toLowerCase().includes(searchQuery.value.toLowerCase())
      );
    });

    const chartData = computed(() => {
      const gradeScores = {};
      students.value.forEach(({ grade, score }) => {
        if (!gradeScores[grade]) gradeScores[grade] = [];
        gradeScores[grade].push(Number(score));
      });

      return Object.entries(gradeScores).map(([grade, scores]) => ({
        label: grade,
        value: scores.reduce((a, b) => a + b, 0) / scores.length,
      }));
    });

    return { students, newStudent, searchQuery, filteredStudents, addStudent, chartData };
  },
};
</script>
