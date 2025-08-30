<script setup>
import { ref, watch } from "vue";
import jsonData from "../assets/response.json";

const props = defineProps({
  selectedChannel: { type: String, default: "" },
});

const positive = ref(0);
const neutral = ref(0);
const negative = ref(0);

const targetValues = ref({ positive: 0, neutral: 0, negative: 0, info: "" });

const animateValue = (refValue, start, end, duration = 800) => {
  const startTime = performance.now();

  const step = (currentTime) => {
    const progress = Math.min((currentTime - startTime) / duration, 1);
    refValue.value = Math.floor(start + (end - start) * progress);

    if (progress < 1) {
      requestAnimationFrame(step);
    }
  };

  requestAnimationFrame(step);
};

watch(
  () => props.selectedChannel,
  (newChannel) => {
    if (newChannel === "all") {
      let totalPos = 0, totalNeu = 0, totalNeg = 0;

      Object.keys(jsonData.data).forEach((channel) => {
        if (channel !== "all" && jsonData.data[channel]?.pie) {
          const [pos, neu, neg] = jsonData.data[channel].pie.series;
          totalPos += pos;
          totalNeu += neu;
          totalNeg += neg;
        }
      });

      targetValues.value = {
        positive: totalPos,
        neutral: totalNeu,
        negative: totalNeg,
        info: "",
      };

      animateValue(positive, positive.value, totalPos);
      animateValue(neutral, neutral.value, totalNeu);
      animateValue(negative, negative.value, totalNeg);
      return;
    }
    const data = jsonData.data[newChannel];
    if (data && data.pie) {
      targetValues.value = {
        positive: data.pie.series[0],
        neutral: data.pie.series[1],
        negative: data.pie.series[2],
        info: "",
      };

      animateValue(positive, positive.value, targetValues.value.positive);
      animateValue(neutral, neutral.value, targetValues.value.neutral);
      animateValue(negative, negative.value, targetValues.value.negative);
    }
  },
  { immediate: true }
);
</script>
<template>
  <div class="row ms-1 mb-3 mt-2">
    <div class="col-12 col-md-4 ">
      <div class="card p-3 text-center h-100">
        <div class="fw-bold fs-4 text-success">
          {{ positive.toLocaleString() }}
        </div>
        <div class="text-muted">Positive</div>
      </div>
    </div>
    <div class="col-12 col-md-4">
      <div class="card p-3 text-center h-100">
        <div class="fw-bold fs-4 text-secondary">
          {{ neutral.toLocaleString() }}
        </div>
        <div class="text-muted">Neutral</div>
      </div>
    </div>
    <div class="col-12 col-md-4">
      <div class="card p-3 text-center h-100">
        <div class="fw-bold fs-4 text-danger">
          {{ negative.toLocaleString() }}
        </div>
        <div class="text-muted">Negative</div>
      </div>
    </div>
  </div>
</template>
