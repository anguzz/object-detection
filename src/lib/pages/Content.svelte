<script>
  import Link from "$lib/components/Link.svelte";
  import Page from "$lib/components/Page.svelte";
  import Text from "$lib/components/Text.svelte";
  import { primaryBackground } from "$lib/utils/constants";
  import {onMount} from "svelte";
  import '@tensorflow/tfjs';
  import * as cocossd from "@tensorflow-models/coco-ssd";
  export let backgroundClass = primaryBackground;

  
  let promise;
  let classify;
  let model;
  let stream;
  let video;

  onMount(() => {
    promise = Promise.all([
      cocossd.load().then((_model) => (model = _model)),
      navigator.mediaDevices.getUserMedia({video: true}).then((_stream) => (stream = _stream)),
    ]);
  });

  function detect(e) {
    classify = model.detect(video).then((detections) => detections.map((detection) => detection.class));
  }

  $: stream, video && (video.srcObject = stream);
</script>

<Page id="content" title=" " {backgroundClass}>
  <Text>
    {#await promise}
    <div class="waviy">
      <span style="--i:1">L</span>
      <span style="--i:2">o</span>
      <span style="--i:3">a</span>
      <span style="--i:4">d</span>
      <span style="--i:5">i</span>
      <span style="--i:6">n</span>
      <span style="--i:7">g</span>
     </div>
  {:then}
    {#await classify}
    {:then result}
      <video autoplay bind:this={video} />
      <button on:click={detect}>Detect</button> 
      {#if result}
      <br><br>
        <div class = "result"> {JSON.stringify(result).replace(/]|[[]/g, '')} </div>
      {:else}{/if}
    {/await}
  {/await}
     
     </Text>
</Page>

<style>

  .result
  {
  	color: #6B91A4;
	background-color: #1F2937;
	outline: none;
  font-size:2rem;
  border-radius: 10px;
  font-weight:400;
  padding:5px;
  }
  button {
	color: #6B91A4;
	background-color: #1F2937;
	outline: none;
  font-size:2rem;
  border-radius: 10px;
  font-weight:400;
  padding:5px;
}
button:hover {
	color: #fbfeff;
	background-color: #3e5fcc;
	outline: none;
  font-size:2rem;
  border-radius: 10px;
  font-weight:400;
  padding:5px;
}


/* --------------------------------------------------loading animation --------------------------------------------------*/

.waviy {
  position: relative;
  -webkit-box-reflect: below -20px linear-gradient(transparent, rgba(0,0,0,.2));
  font-size: calc(1.5rem + 3vw);
}
.waviy span {
  position: relative;
  display: inline-block;
  color: rgb(131, 196, 250);
  text-transform: uppercase;
  animation: waviy 1s infinite;
  animation-delay: calc(.1s * var(--i));
  
}
@keyframes waviy {
  0%,40%,100% {
    transform: translateY(0)
  }
  20% {
    transform: translateY(-10px)
  }
}
</style>

