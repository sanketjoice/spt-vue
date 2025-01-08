<template>
  <div class="bg-white p-6 rounded-lg shadow-md">
    <canvas ref="canvas" class="w-full"></canvas>
  </div>
</template>

<script>
import { onMounted, ref, watch } from 'vue';
import Chart from 'chart.js/auto';

export default {
  props: {
    data: {
      type: Array,
      required: true,
    }
  },
  setup(props) {
    const canvas = ref(null);
    let chartInstance = null;

    const renderChart = () => {
      if (chartInstance) {
        chartInstance.destroy(); 
      }

      chartInstance = new Chart(canvas.value, {
        type: "bar",
        data: {
          labels: props.data.map((item) => item.label),
          datasets: [
            {
              label: "Average Score",
              data: props.data.map((item) => item.value),
              backgroundColor: [
                "rgba(54, 162, 235, 0.6)",
                "rgba(255, 99, 132, 0.6)",
                "rgba(255, 206, 86, 0.6)",
                "rgba(75, 192, 192, 0.6)",
                "rgba(153, 102, 255, 0.6)",
              ],
              borderWidth: 1,
            },
          ],
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            y: {
              beginAtZero: true,
            },
          },
        },
      });
    };

    onMounted(() => {
      if (props.data.length > 0) {
        renderChart();
      }
    });

    watch(() => props.data, (newData, oldData) => {
      if (newData.length > 0) {
        renderChart();
      }
    });

    return { canvas };
  },
};
</script>
