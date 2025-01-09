<template>
  <div class="bg-white p-6 rounded-lg shadow-md">
    <canvas ref="canvas" class="w-full"></canvas>
  </div>
  <!-- The canvas element where the chart will be rendered -->
</template>

<script>
import { onMounted, ref, watch } from 'vue';
  // Import the Chart.js library
import Chart from 'chart.js/auto';
// Define the component name and expected props (labels and data)
export default {
  props: {
    data: {
      type: Array,
      required: true,
    }
  },
  // Initialize the chart instance variable (used to store the current chart)

  setup(props) {
    const canvas = ref(null);
    let chartInstance = null;

// Function to render the chart on the canvas
    const renderChart = () => {
      if (chartInstance) {
        chartInstance.destroy(); 
        // Destroy the previous chart instance if it exists to avoid duplicates

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
      // Renders the chart when the component is mounted
      if (props.data.length > 0) {
        renderChart();
      }
    });

    watch(() => props.data, (newData, oldData) => {

      // Re-renders the chart whenever the data prop changes
        if (newData.length > 0) {
        renderChart();
      }
    });

    return { canvas };
  },
};
</script>
