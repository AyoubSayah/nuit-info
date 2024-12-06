<template>
    <div class="shot-container" @click="fireShot">
      <div 
        v-if="showShot" 
        :style="{ left: shotX + 'px', top: shotY + 'px' }" 
        class="shot">
      </div>
      <button class="fire-btn">Click to Fire</button>
    </div>
  </template>
  <script>
  export default {
    data() {
      return {
        showShot: false,
        shotX: 0,
        shotY: 0,
      };
    },
    methods: {
      fireShot(event) {
        const rect = event.currentTarget.getBoundingClientRect();
        this.shotX = event.clientX - rect.left;
        this.shotY = event.clientY - rect.top;
  
        this.showShot = true;
        setTimeout(() => {
          this.showShot = false;
        }, 300); 
      },
    },
  };
  </script>
  <style scoped>
  .shot-container {
    position: relative;
    width: 100%;
    height: 300px;
    border: 1px solid #ddd;
    background: #f4f4f4;
    cursor: crosshair;
  }
  
  .shot {
    position: absolute;
    width: 10px;
    height: 10px;
    background: red;
    border-radius: 50%;
    animation: shotEffect 0.3s ease-out;
  }
  
  .fire-btn {
    position: absolute;
    bottom: 10px;
    left: 50%;
    transform: translateX(-50%);
    padding: 10px 20px;
    font-size: 16px;
  }
  
  @keyframes shotEffect {
    0% {
      transform: scale(1);
      opacity: 1;
    }
    100% {
      transform: scale(3);
      opacity: 0;
    }
  }
  </style>
  