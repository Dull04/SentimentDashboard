

<script setup>
import { onMounted, watch } from "vue";
import { Chart } from "chart.js/auto";
import ChartDataLabels from "chartjs-plugin-datalabels";

Chart.register(ChartDataLabels);

const props = defineProps({
  selectedChannel: {
    type: String,
    default: "all",
  },
  responseData: {
    type: Object,
    required: true,
  },
});

let chartInstance = null;
const renderChart = () => {
  const ctx = document.getElementById("barChart");
  let chartData = {};
  let chartOptions = {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
      legend: { position: "top" },
      datalabels: {},
      tooltip: {
        enabled: true,
        callbacks: {
          label: (context) => {
            const dataset = context.chart.data.datasets;
            const dataIndex = context.dataIndex;
            if (props.selectedChannel === "all") {
              let lines = dataset.map(
                (d) => `${d.label}: ${d.data[dataIndex].toLocaleString()}`
              );
              let total = dataset.reduce(
                (acc, d) => acc + d.data[dataIndex],
                0
              );
              lines.push(`Total: ${total.toLocaleString()}`);
              return lines;
            } else {
              return `${context.dataset.label}: ${context.formattedValue}`;
            }
          },
        },
      },
    },
  };

  if (props.selectedChannel === "all") {
    const col = props.responseData.all.column;
    chartData = {
      labels: col.categories,
      datasets: col.series.map((s, i) => ({
        label: s.name,
        data: s.data,
        backgroundColor: col.color[i],
        categoryPercentage: 1,
        barPercentage: 0.9,
        borderRadius: 20,
      })),
    };
    chartOptions.scales = { x: { stacked: true }, y: { stacked: true } };
    chartOptions.plugins.datalabels.display = false;
  } else {
    const bar = props.responseData[props.selectedChannel].bar;
    chartData = {
      labels: bar.categories,
      datasets: [
        {
          label: bar.series[0].name,
          data: bar.series[0].data.map((d) => d.y),
          backgroundColor: bar.series[0].data.map((d) => d.color),
          categoryPercentage: 0.8,
          barPercentage: 0.9,
          borderRadius: 20,
        },
      ],
    };
    chartOptions.plugins.legend.display = false;
    chartOptions.plugins.datalabels = {
      anchor: "center",
      align: "center",
      color: "#000",
      font: { weight: "bold", size: 12 },
      formatter: (value) => value.toLocaleString(),
    };
  }

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
watch(() => props.selectedChannel, renderChart);
</script>

<template>
  <div
    class="card p-3 mt-3 mb-3 d-flex align-items-center justify-content-center barflex1"
  >
    <div class="w-100 position-relative barheight1">
      <div
        v-if="selectedChannel === 'all'"
        class="position-absolute top-0 end-0 me-2 mt-2 text-muted small fst-italic"
      >
        Hover to view details
      </div>

      <div class="d-flex align-items-center justify-content-center h-100">
        <canvas id="barChart"></canvas>
      </div>
    </div>
  </div>
</template>
