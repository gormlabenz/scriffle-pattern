<template>
  <form action="#" class="w-full h-16 bg-gray-200 px-4 py-2 hidden">
    <input
      class="text-xl"
      id="image-upload"
      type="file"
      @change="createUrls"
      multiple
    />
    <input class="text-xl" type="text" v-model="imageCountRaw" />
    <button @click="download">Download</button>
  </form>
  <div id="pattern" class="flex flex-wrap">
    <img
      class="cursor-pointer"
      v-for="(image, i) in imageSrcRandom"
      v-bind:key="i"
      :id="i"
      @click="setRandomImageToSrc(i)"
      :src="image"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from "vue";
import { toPng } from "html-to-image";

export default defineComponent({
  name: "Menu",
  setup() {
    let imageCountRaw = ref<string>("240");
    let imageCount = computed(() => {
      const int = parseInt(imageCountRaw.value.replace(/\D/g, ""));

      return isNaN(int) ? 0 : int;
    });

    interface Event<T = EventTarget> {
      target: T;
    }
    const imageSrc = ref<string[]>([]);

    function createUrls(event: Event<HTMLInputElement>) {
      if (event.target.files !== null) {
        const files = event.target.files; // FileList object
        for (let i = 0; i < files.length; ++i) {
          imageSrc.value.push(URL.createObjectURL(files[i]));
        }
      }
      console.log({ imageSrc });
    }

    const imageSrcRandom = computed(() => {
      return Array.from({ length: imageCount.value }, () => {
        return getRandomImage();
      });
    });

    const getRandomImage = () =>
      imageSrc.value[Math.floor(Math.random() * imageSrc.value.length)];

    const setRandomImageToSrc = (id: string) => {
      const image = getRandomImage();
      let el = document.getElementById(id) as HTMLImageElement;
      el.src = image;
    };

    async function download() {
      const patternEl = document.getElementById("pattern");
      if (patternEl) {
        const dataURL = await toPng(patternEl);
        console.log("downloadinf");
        /* eslint-disable */
        // @ts-ignore
        download(dataURL, 'pattern.png') 
        /* eslint-enable */
      }
    }

    return {
      createUrls,
      download,
      setRandomImageToSrc,
      imageSrcRandom,
      imageCountRaw,
      imageCount,
    };
  },
});
</script>
