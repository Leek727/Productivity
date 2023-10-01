<script lang="js">
  import { onMount } from "svelte";
  import { tweened } from "svelte/motion";

  let title = `0:0`;
  // Function to update the title
  const updateTitle = () => {
    document.title = title;
  };

  let cycles = 0;
  let times = [25*60, 5 * 60, 25 * 60, 5 * 60,
              25 * 60, 5 * 60, 25 * 60,
              25 * 60];
  let cycle_names = ["Work", "Rest", "Work", "Rest", "Work", "Rest", "Work", "Rest", "Long Rest"]

  let i = 0;
  let original = times[i];
  let timer = tweened(original);

  // ------ dont need to modify code below
  setInterval(() => {
    if ($timer > 0) $timer--;
    title = `${Math.floor($timer / 60)}:${Math.floor(
      $timer - Math.floor($timer / 60) * 60
    )}`;
    updateTitle();
  }, 1000);

  $: minutes = Math.floor($timer / 60);
  $: seconds = Math.floor($timer - minutes * 60);

  // play alarm audio

  $: if (minutes < 0 || seconds < 0) {
    minutes = 0;
    seconds = 0;
  }

  let audio;
  $: if ($timer <= 0) {
    audio.play();
  }

  function nextTime() {
    if ($timer <= 0) {
      audio.pause();
      i++;
      timer = tweened(times[i % times.length]);
    }
  }

  function skip() {
    audio.pause();
    i++;
    timer = tweened(times[i % times.length]);
  }

  $: current_cycle = cycle_names[i];
</script>

<svelte:head>
  <title>{title}</title>
</svelte:head>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
  on:click={nextTime}
  class="flex bg-gradient-to-r from-violet-500 to-fuchsia-500 w-screen h-screen"
>
  <div class="m-auto">
    <h1 class="text-7xl pb-10 text-teal-800 ">
      {current_cycle}
    </h1>
    <h1 class="text-[200px] pb-10 text-slate-700">
      {minutes}:{seconds}
    </h1>
    <button
      on:click={skip}
      class="text-4xl bg-transparent hover:bg-fuchsia-500 text-slate-700 font-semibold hover:text-slate-700 py-4 px-8 border-2 border-slate-700 hover:border-transparent rounded"
    >
      Skip
    </button>
    <audio
      src="https://www.chosic.com/wp-content/uploads/2021/04/burghrecords__birds-singing-forest-scotland(chosic.com).mp3"
      bind:this={audio}
    />
  </div>
</div>
