<script lang="ts">
  import { ImageStatus } from "../types.d"
  import CloudinaryLogo from "./CloudinaryLogo.svelte"
  import StepUpload from "./StepUpload.svelte"
  import StepEdit from "./StepEdit.svelte"
  import { imageStatus } from "./store.js"
</script>

<div class="grid grid-cols-1 max-w-xl m-auto place-content-center w-full h-screen p-4">

  <header class="fle justify-center py-10">
    <h1 class="text-3xl font-bold text-blue-900 tracking-tighter">Remove 
      <span class="text-blue-600">BG</span></h1>
  </header>

  <main class="w-full block">
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit />
    {/if}
  </main>

  <footer class="flex justify-center items-center gap-x-2 font-semibold pt-10">
  Hecho con <a href="https://cloudinary.com/" target="_blank" rel="noreferrer"><CloudinaryLogo /></a>
</footer>

</div>

