<script>
  import Link from "$lib/components/Link.svelte";
  import Page from "$lib/components/Page.svelte";
  import Text from "$lib/components/Text.svelte";
  import { primaryBackground } from "$lib/utils/constants";

  export let backgroundClass = primaryBackground;
  import {onMount} from "svelte";

  import '@tensorflow/tfjs';
  import * as cocossd from "@tensorflow-models/coco-ssd";



  let promise;
  let model;
  let webcam;
  let video;
  let classify;

 
</script>

<Page id="content" title=" " {backgroundClass}>
  <Text>
  
     
     </Text>
</Page>


<style>
  
</style>

