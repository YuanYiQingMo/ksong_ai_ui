<template>
    <div>
      <h2>图像坐标展示</h2>
      <div>
        <label for="startX">起始点 X 坐标:</label>
        <input v-model="point1.x" type="number" id="startX" readonly />
        <label for="startY">起始点 Y 坐标:</label>
        <input v-model="point1.y" type="number" id="startY" readonly />
      </div>
      <div>
        <label for="endX">结束点 X 坐标:</label>
        <input v-model="point2.x" type="number" id="endX" readonly />
        <label for="endY">结束点 Y 坐标:</label>
        <input v-model="point2.y" type="number" id="endY" readonly />
      </div>
      <div>
        <label for="mark1">请输入起始点在坐标轴中的实际横坐标：</label>
        <input v-model="point3" type="number" id="v1" placeholder="输入横坐标" />
      </div>
      <div>
        <label for="mark2">请输入终止点在坐标轴中的实际横坐标：</label>
        <input v-model="point4" type="number" id="v2" placeholder="输入横坐标" />
      </div>
  
      <div style="position: relative;">
        <img
          src="@/assets/image2.png"  
          @click="setCoordinates"
          @mousemove="updateMouseCoordinates"  
          style="max-width: 100%; cursor: pointer;"
        />
        <div
          v-if="showingCoords"
          class="coordinates"
          :style="{ left: `${mouseX}px`, top: `${mouseY}px` }"
        >
          X: {{ computedX }}, Y: {{ mouseY }}
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue';

  
  const point1 = ref({ x: 0, y: 0 });   
  const point2 = ref({ x: 0, y: 0 });   
  const point3 = ref(0); 
  const point4 = ref(0); 
  const mouseX = ref(0);
  const mouseY = ref(0);
  const showingCoords = ref(false);
  
  
  const computedX = computed(() => {
    return mouseX.value * (point3.value - point4.value) / (point1.value.x - point2.value.x);
  });
  
  function setCoordinates(event) {
    const rect = event.target.getBoundingClientRect();
  
    
    const x = Math.round(event.clientX - rect.left);
    const y = Math.round(event.clientY - rect.top);
  
    if (point1.value.x === 0 && point1.value.y === 0) {
      point1.value.x = x;
      point1.value.y = y;
    } else {
      point2.value.x = x;
      point2.value.y = y;
    }
    
    showingCoords.value = true;
  }
  
  function updateMouseCoordinates(event) {
    const rect = event.target.getBoundingClientRect();
    mouseX.value = Math.round(event.clientX - rect.left); 
    mouseY.value = Math.round(event.clientY - rect.top);  
    showingCoords.value = true; 
  }
  </script>
  
  <style scoped>
  .coordinates {
    position: absolute;
    top: 10px; 
    left: 10px;
    background-color: rgba(0, 0, 0, 0.7);
    color: white;
    padding: 8px;
    border-radius: 4px;
    pointer-events: none; 
  }
  </style>