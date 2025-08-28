<script setup>
import { ref, defineEmits } from "vue";

const emits = defineEmits(["update:selectedChannel"]);

// Tambahin "all" lagi di awal
const channels = ["all", "news", "instagram", "youtube", "facebook", "tiktok", "twitter"];
const selected = ref(""); 

const handleChange = () => {
  emits("update:selectedChannel", selected.value);
};
</script>

<template>
  <div class="d-flex align-items-center mb-2 w-100">
    <div class="d-flex align-items-center">
      <label for="channelSelect" class="me-2 ms-3">Filter Channel</label>
      <select
        id="channelSelect"
        class="form-select w-auto"
        v-model="selected"
        @change="handleChange"
      >
        <option disabled value="">
          -- Pilih Channel --
        </option>
        <option
          v-for="channel in channels"
          :key="channel"
          :value="channel"
        >
          {{ channel === "all" ? "Semua Channel" : channel.charAt(0).toUpperCase() + channel.slice(1) }}
        </option>
      </select>
    </div>
    <div class="ms-auto me-5">
      Welcome, User
    </div>
  </div>
</template>
