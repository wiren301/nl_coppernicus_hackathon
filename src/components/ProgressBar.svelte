<script>
    import { onMount } from 'svelte';
    
    export let duration = 30000; // default to 5 seconds
    
    let progress = 0;
    let timer;

    export function startProgress() {
      progress = 0;
      clearInterval(timer);
      const increment = 0.1; // 0.1% increments
      const interval = duration / (100 / increment); // adjusted interval for smooth progress
      timer = setInterval(() => {
        progress += increment;
        if (progress >= 100) {
          clearInterval(timer);
        }
      }, interval);
    }
</script>

<style>
    .progress-container {
      width: 100%;
      background-color: #f3f3f3;
      border-radius: 4px;
    }

    .progress-bar {
      height: 30px;
      width: 0;
      background-color: #4CAF50;
      border-radius: 4px;
      text-align: center;
      line-height: 30px;
      color: white;
      transition: width 0.1s linear; /* smooth transition for width */
    }
</style>

<div class="progress-container ">
    <div class="progress-bar" style="width: {progress.toFixed(1)}%"> <!-- Rounded to 1 decimal place -->
    </div>
</div>
