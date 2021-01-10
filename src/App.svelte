<script lang="typescript">
  import { onMount } from "svelte";
  import Bar from "./components/Bar";

  let midiAccessStatus: "pending" | "approved" | "denied" | "unsupported" =
    "pending";

  let midiInputs;
  let midiOutputs;
  onMount(() => {
    if (navigator.requestMIDIAccess) {
      navigator.requestMIDIAccess().then(
        (midiAccess) => {
          midiAccessStatus = "approved";
          midiInputs = midiAccess.inputs;
          midiOutputs = midiAccess.outputs;
          (window as any).inputs = midiInputs;
        },
        () => {
          midiAccessStatus = "denied";
        }
      );
    } else {
      midiAccessStatus = "unsupported";
    }
  });
</script>

<div>
  {#if midiAccessStatus === "pending"}
    Requesting access to WebMIDIApi
  {:else if midiAccessStatus === "approved"}
    <Bar />
  {:else if midiAccessStatus === "denied"}
    Permission denied
  {:else if midiAccessStatus === "unsupported"}
    WebMIDIApi not supported by your browser
  {/if}
</div>

<style>
</style>
