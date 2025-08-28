<script setup>
import { ref, defineEmits } from "vue";

const emits = defineEmits(["update:selectedChannel"]);

const channels = ["all", "news", "instagram", "youtube", "facebook", "tiktok", "twitter"];
const selected = ref("all");

const handleChange = () => {
  emits("update:selectedChannel", selected.value);
};
</script>

<template>
  <div class="d-flex align-items-center mb-2 w-50">
    <div class="d-flex align-items-center">
      <label for="channelSelect" class="me-2">Filter Channel</label>
      <select id="channelSelect" class="form-select w-auto" v-model="selected" @change="handleChange">
        <option v-for="channel in channels" :key="channel" :value="channel">
          {{ channel.charAt(0).toUpperCase() + channel.slice(1) }}
        </option>
      </select>
    </div>

    <!-- Welcome di kanan -->
    <div class="ms-auto text-center w-50">
      Welcome, User
    </div>
  </div>
</template>

