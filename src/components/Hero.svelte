<script lang="js">
  import { fade } from 'svelte/transition';
  import { onMount } from 'svelte';

  function focusNext() {
    const el = document.getElementById("demo");
    el.scrollIntoView({behavior: "smooth"});
  }
  
  let ready = false;
  onMount(() => {
      ready = true;  
  });
</script>

<section class="banner">
  <div class="bg-black w-screen h-screen overflow-hidden">
      
      <video class="opacity-40" autoplay muted loop id="banner_video">
          <source src="hero/video.mp4" type="video/mp4" class="">
      </video>

      {#if ready}
          <div in:fade={{duration: 1000}} class="absolute top-1/2 left-1/2 -translate-x-[50%] -translate-y-[50%] text-gray-300 text-center lg:p-6 p-4 border-4  border-gray-300 pb-2 ">
              <h1 class="text-3xl lg:text-[5rem]"> flood watch </h1>
              <h3 class="text-right text-sm lg:text-xl lg:translate-y-5 lg:translate-x-3">because we care.</h3>
          </div>
          
          <button 
            class="w-10 h-10 absolute bottom-4 right-1/2 translate-x-[50%] rounded-full border-1 svgToWhite border-black focus:outline-none" 
            on:click={focusNext} 
    
          >
            <img in:fade={{duration: 1000}} src="swipe-down.png" alt="Swipe down" />
          </button>
        {/if}
  </div>
</section>
