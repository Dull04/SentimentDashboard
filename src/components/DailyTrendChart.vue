<script setup>
import { onMounted } from "vue";
import { Chart } from "chart.js/auto";
import ChartDataLabels from "chartjs-plugin-datalabels";
import jsonData from "../assets/response.json";

Chart.register(ChartDataLabels);

const histogram = jsonData.data.all.histogram;

let chartInstance = null;

const renderChart = () => {
  const ctx = document.getElementById("dailyTrendChart");

  const chartData = {
    labels: histogram.categories,
    datasets: histogram.series.map((s) => ({
      label: s.name,
      data: s.data,
      backgroundColor:
        s.name === "Positive"
          ? "#3EC764"
          : s.name === "Neutral"
          ? "#B3B6C6"
          : "#ED3E3E",
      borderRadius: 6,
    })),
  };

  const chartOptions = {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
      legend: {
        position: "top",
        labels: { font: { size: 12 } },
      },
      tooltip: {
        enabled: true,
        mode: "index",
        intersect: false,
        callbacks: {
          label: (context) => {
            let value = context.raw.toLocaleString("id-ID");
            return `${context.dataset.label}: ${value}`;
          },
          footer: (tooltipItems) => {
            let total = tooltipItems.reduce((sum, item) => sum + item.raw, 0);
            return "Total: " + total.toLocaleString("id-ID");
          },
        },
      },
      datalabels: { display: false },
    },
    scales: {
      x: { stacked: true, ticks: { font: { size: 10 } } },
      y: { stacked: true, beginAtZero: true, ticks: { font: { size: 10 } } },
    },
  };

  if (chartInstance) {
    chartInstance.data = chartData;
    chartInstance.options = chartOptions;
    chartInstance.update();
    return;
  }

  chartInstance = new Chart(ctx, {
    type: "bar",
    data: chartData,
    options: chartOptions,
  });
};

onMounted(renderChart);
</script>

<template>
  <div class="card p-0 h-100 mb-3 ms-3">
    <div class="card-body p-3 d-flex flex-column">
      <h5 class="card-title m-0 ms-3">Daily Trend</h5>
      <div class="flex-grow-1 d-flex align-items-center dailychart">
        <div class="w-100 position-relative barheight">
          <canvas id="dailyTrendChart"></canvas>
        </div>
      </div>
    </div>
  </div>
</template>



