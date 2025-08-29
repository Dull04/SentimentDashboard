<script setup>
import { Chart as ChartJS, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from "chart.js";
import { Bar } from "vue-chartjs";
import { computed } from "vue";
import jsonData from "../assets/response.json";

// Register Chart.js komponen
ChartJS.register(Tooltip, Legend, BarElement, CategoryScale, LinearScale);

// Ambil data histogram dari response.json
const histogram = jsonData.data.all.histogram;

// Data untuk chart
const chartData = computed(() => ({
  labels: histogram.categories, // tanggal
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
}));

// Opsi chart
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
        label: function (context) {
          let value = context.raw.toLocaleString("id-ID");
          return `${context.dataset.label}: ${value}`;
        },
        footer: function (tooltipItems) {
          let total = tooltipItems.reduce((sum, item) => sum + item.raw, 0);
          return "Total: " + total.toLocaleString("id-ID");
        },
      },
    },
    datalabels: {
      display: false, // 🔹 matiin angka di dalam bar
    },
  },
  scales: {
    x: {
      stacked: true,
      ticks: { font: { size: 10 } },
    },
    y: {
      stacked: true,
      beginAtZero: true,
      ticks: { font: { size: 10 } },
    },
  },
};
</script>

<template>
  <div style="height: 250px;">
    <Bar :data="chartData" :options="chartOptions" />
  </div>
</template>
