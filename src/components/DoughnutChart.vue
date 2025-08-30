<script setup>
import { computed } from "vue";
import { Chart as ChartJS, Title, Tooltip, Legend, ArcElement } from "chart.js";
import ChartDataLabels from "chartjs-plugin-datalabels";
import { Doughnut } from "vue-chartjs";
import response from "../assets/response.json";

ChartJS.register(Title, Tooltip, Legend, ArcElement, ChartDataLabels);

const props = defineProps({
  selectedChannel: {
    type: String,
    default: "",
  },
});

const colors = ["#3EC764", "#B3B6C6", "#ED3E3E"];

const chartData = computed(() => {
  if (!props.selectedChannel) return null;

  let channelData = null;
  if (props.selectedChannel === "all") {
    channelData = response.data.all.pie;
  } else {
    channelData = response.data[props.selectedChannel]?.pie;
  }

  if (!channelData) return null;

  const total = channelData.series.reduce((a, b) => a + b, 0);

  return {
    categories: channelData.categories,
    values: channelData.series,
    total,
    chart: {
      labels: channelData.categories,
      datasets: [
        {
          data: channelData.series,
          backgroundColor: colors,
        },
      ],
    },
  };
});

const chartOptions = computed(() => ({
  responsive: true,
  maintainAspectRatio: false,
  cutout: "60%",
  plugins: {
    legend: { display: false },
    title: {
      display: true,
      text:
        props.selectedChannel === "all"
          ? "Distribusi Sentiment Semua Channel"
          : `Distribusi Sentiment ${
              props.selectedChannel.charAt(0).toUpperCase() +
              props.selectedChannel.slice(1)
            }`,
    },
    datalabels: {
      color: "#000",
      font: { weight: "bold" },
      formatter: (value, ctx) => {
        const dataArr = ctx.chart.data.datasets[0].data;
        const total = dataArr.reduce((a, b) => a + b, 0);
        return ((value / total) * 100).toFixed(1) + "%";
      },
    },
  },
}));

const placeholderData = {
  labels: ["Belum ada data"],
  datasets: [
    {
      data: [1],
      backgroundColor: ["#B3B6C6"],
    },
  ],
};

const placeholderOptions = {
  responsive: true,
  maintainAspectRatio: false,
  cutout: "60%",
  plugins: {
    legend: { display: false },
    title: { display: false },
    datalabels: { display: false },
  },
};
</script>

<template>
  <div
    class="card p-3 d-flex align-items-center justify-content-center mt-3 flex-1"
  >
    <div class="chart-wrapper">
      <div class="chart-container">
        <Doughnut
          v-if="chartData"
          :data="chartData.chart"
          :options="chartOptions"
        />
        <Doughnut
          v-else
          :data="placeholderData"
          :options="placeholderOptions"
        />
      </div>

      <div
        v-if="chartData"
        class="chart-legend d-flex justify-content-center mt-3 gap-4 flex-wrap"
      >
        <div
          v-for="(label, i) in chartData.categories"
          :key="i"
          class="legend-item d-flex align-items-center"
        >
          <span
            class="legend-color"
            :style="{ backgroundColor: colors[i] }"
          ></span>
          <span>
            {{ label }}: {{ chartData.values[i] }} ({{
              ((chartData.values[i] / chartData.total) * 100).toFixed(1)
            }}%)
          </span>
        </div>
      </div>

      <div v-else class="text-muted text-center mt-2">
        Silakan pilih satu channel untuk menampilkan chart
      </div>
    </div>
  </div>
</template>
