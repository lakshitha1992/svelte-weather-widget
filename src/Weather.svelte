<script>
    import { onMount } from 'svelte';
  
    let weatherType = 'sun'; // Initial weather type
  
    // Settings for animation
    let settings = {
      windSpeed: 2,
      rainCount: 0,
      leafCount: 0,
      snowCount: 0,
      cloudHeight: 100,
      cloudSpace: 30,
      cloudArch: 50
    };
  
    // Function to handle weather changes
    function changeWeather(type) {
      weatherType = type;
    }
  
    onMount(() => {
      if (weatherType === 'rain') {
        makeRain();
      }
    });
  
    // Create rain animation
    function makeRain() {
      let container = document.getElementById('rain-container');
      let numRaindrops = 50;
      
      for (let i = 0; i < numRaindrops; i++) {
        let rainDrop = document.createElement('div');
        rainDrop.classList.add('rain');
        rainDrop.style.left = `${Math.random() * 100}%`;
        rainDrop.style.animationDuration = `${Math.random() * 2 + 1}s`;
        container.appendChild(rainDrop);
      }
    }
  </script>
  
  <style>
    .container {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #eeeeee;
      overflow: hidden;
      position: relative;
    }
  
    .weather-card {
      background-color: #fff;
      border-radius: 10px;
      width: 300px;
      height: 400px;
      display: flex;
      justify-content: center;
      align-items: center;
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
      overflow: hidden;
      position: relative;
    }
  
    .buttons {
      position: absolute;
      top: 20px;
      left: 20px;
      z-index: 10;
    }
  
    .buttons button {
      margin-right: 10px;
      padding: 10px;
      background-color: #007bff;
      color: #fff;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
  
    .rain {
      position: absolute;
      top: -10px;
      width: 2px;
      height: 10px;
      background-color: rgba(0, 0, 255, 0.7);
      animation: rain-animation linear infinite;
    }
  
    @keyframes rain-animation {
      to {
        transform: translateY(100vh);
      }
    }
  
    .sunny {
      background-color: #f0e68c;
    }
  
    .rainy {
      background-color: #7ec8f1;
    }
  
    .clouds {
      width: 100%;
      height: 100%;
      position: absolute;
      background: url('https://i.imgur.com/kh0eUOw.png') repeat-x;
      background-size: 100px 60px;
      top: 0;
      animation: clouds 10s linear infinite;
    }
  
    @keyframes clouds {
      from {
        left: 0;
      }
      to {
        left: 100%;
      }
    }
  </style>
  
  <div class="container">
    <div class="weather-card {weatherType}">
      {#if weatherType === 'rain'}
        <div id="rain-container" class="rain-container"></div>
      {/if}
      {#if weatherType === 'sun'}
        <div class="sunny">☀️</div>
      {/if}
      {#if weatherType === 'wind'}
        <div class="clouds"></div>
      {/if}
      {#if weatherType === 'snow'}
        <div class="snow"></div>
      {/if}
    </div>
  
    <div class="buttons">
      <button on:click={() => changeWeather('sun')}>Sunny</button>
      <button on:click={() => changeWeather('rain')}>Rain</button>
      <button on:click={() => changeWeather('wind')}>Windy</button>
      <button on:click={() => changeWeather('snow')}>Snow</button>
    </div>
  </div>
  