<script>
  import Pomodoro from "./lib/Pomodoro.svelte";

  let started = false;
</script>

{#if started}
  <Pomodoro />
{:else}
  <div
    class="flex bg-gradient-to-r from-violet-500 to-fuchsia-500 w-screen h-screen"
  >
    <div class="m-auto">
      <button
        on:click={() => {
          started = true;
        }}
        class="text-4xl bg-transparent hover:bg-fuchsia-500 text-slate-700 font-semibold hover:text-slate-700 py-4 px-8 border-2 border-slate-700 hover:border-transparent rounded"
      >
        Start
      </button>
    </div>
  </div>
{/if}
