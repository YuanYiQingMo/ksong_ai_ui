<template>
  <v-row>
    <v-card class="ai-card">
      {{ data.text }}
    </v-card>
  </v-row>
</template>
<script setup>
defineProps(["data"]);
</script>

<style scoped>
.ai-text {
  font: bold;
}
.ai-card {
  width: 80%;
  margin: 24px 0;
}
</style>
