<svelte:head>
	<title>{title}</title>
</svelte:head>

<script lang="js">
  import { onMount } from "svelte";
  import { tweened } from "svelte/motion";

  let title = `0:0`;
  // Function to update the title
  const updateTitle = () => {
    document.title = title;
  };


  let cycles = 0;
  let times = [
    5,
    5 * 60,
    25 * 60,
    5 * 60,

    25 * 60,
    5 * 60,
    25 * 60,

    25 * 60,
  ];

  let i = 0;
  let original = times[i];
  let timer = tweened(original);

  // ------ dont need to modify code below
  setInterval(() => {
    if ($timer > 0) $timer--;
    title = `${Math.floor($timer / 60)}:${Math.floor($timer - minutes * 60)}`;
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

</script>


<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div
  on:click={nextTime}
  class="flex bg-gradient-to-r from-violet-500 to-fuchsia-500 w-screen h-screen"
>
  <div class="m-auto">
    <h1 class="text-[200px] pb-10 text-slate-700">
      {minutes}:{seconds}
    </h1>

    <audio
      src="https://www.chosic.com/wp-content/uploads/2021/04/burghrecords__birds-singing-forest-scotland(chosic.com).mp3"
      bind:this={audio}
    />
  </div>
</div>
