<script lang="ts">
  import Dropzone from 'dropzone'
  import 'dropzone/dist/dropzone.css'
  import {onMount} from 'svelte'
  import { ImageStatus } from "../types.d"
  import { imageStatus, modifiedImage, originalImage } from "./store.js"
  import { Cloudinary } from "@cloudinary/url-gen"
  import { backgroundRemoval } from "@cloudinary/url-gen/actions/effect"

  const cloudinary = new Cloudinary({
    cloud: {
      cloudName: "dxac91peg",
    },
    url: {
      secure: true,
    },
  })

onMount( () => {
  const dropzone = new Dropzone("#dropzone", {
      uploadMultiple: false,
      acceptedFiles: ".jpg,.png,.webp",
      maxFiles: 1,
    })

  dropzone.on("sending", (file, xhr, formData) => {
    imageStatus.set(ImageStatus.UPLOADING)
    formData.append("upload_preset", "edu_web")
    formData.append("timestamp", Date.now() / 1000)
    formData.append("api_key", 477411338397295)
  })

  dropzone.on("success", (file, response) => {
      const { public_id: publicId, secure_url: url } = response

      // crear la imagen con fondo transparente
      // y guardar en el backgroundImage
      const imageWithoutBackground = cloudinary
        .image(publicId)
        .effect(backgroundRemoval())

      console.log(imageWithoutBackground.toURL())

      imageStatus.set(ImageStatus.DONE)
      modifiedImage.set(imageWithoutBackground.toURL())
      originalImage.set(url)
    })

  dropzone.on('error', (file, response) => {
    console.log('error')
    console.log(response)
  })

})
</script>

<form id="dropzone" action="https://api.cloudinary.com/v1_1/dxac91peg/image/upload" 
class='shadow-xl border-dashed border-2 border-gray-300 rounded-lg aspect-video w-full flex items-center justify-center flex-col'>

{#if $imageStatus === ImageStatus.READY}
<button
  class="font-bold pointer-events-none bg-blue-600 rounded-full text-bold text-white text-xl px-6 py-4"
>
  Upload files
</button>
<strong class="text-lg mt-4 text-gray-800">or drop a file</strong>
{:else if $imageStatus === ImageStatus.UPLOADING}
<strong class="text-lg mt-4 text-gray-800">Uploading files...</strong>
{/if}

</form>