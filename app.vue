<script setup>
import BigBuckBunny from "big-buck-bunny-1080p/video.mp4";
const videoSource = ref();
const imageCanvas = ref();
const image = ref();
const image2 = ref();
const imageToggle = ref(true);

const timer = ref();

function createImage() {
  imageCanvas.value.height = videoSource.value.clientHeight;
  imageCanvas.value.width = videoSource.value.clientWidth;

  const context = imageCanvas.value.getContext("2d");

  context.drawImage(
    videoSource.value,
    0,
    0,
    videoSource.value.clientWidth,
    videoSource.value.clientHeight
  );

  if (!imageToggle.value) {
    image.value = imageCanvas.value.toDataURL("image/jpeg");
  } else {
    image2.value = imageCanvas.value.toDataURL("image/jpeg");
  }

  imageToggle.value = !imageToggle.value;
}

function startTimer() {
  setTimeout(() => {
    createImage();
    timer.value = setInterval(() => {
      createImage();
    }, 12000);
  }, 500);
}

function stopTimer() {
  if (timer.value) 
clearInterval(timer.value);
}

onMounted(() => {
  videoSource.value.play()
})
</script>

<template>
  <div class="h-full min-h-screen w-full bg-neutral-900 p-4">
    <div class="my-8 flex flex-col items-center justify-center text-center">
      <p
        class="flex items-center justify-center gap-2 text-2xl font-bold text-neutral-100 lg:text-3xl"
      >
        <span><Icon name="logos:youtube-icon" size="2.4rem" /></span>YouTube
      </p>
      <p class="text-3xl font-bold text-neutral-100 lg:text-6xl">
        Ambient Mode
      </p>
      <p class="text-md font-semibold text-neutral-200 lg:text-2xl">
        Recreated with Nuxt 3 and TailwindCSS
      </p>
    </div>
    <div
      class="my-8 mx-auto grid h-full w-full place-items-center bg-transparent lg:p-8"
    >
      <div class="relative isolate lg:max-w-6xl">
        <video
          ref="videoSource"
          class="aspect-video rounded-xl"
          :src="BigBuckBunny"
          playsinline
          muted
          loop
          controls
          @play="startTimer"
          @pause="stopTimer"
        ></video>

        <div
          class="opacity-65 pointer-events-none absolute inset-0 z-[-1] rounded-xl blur-xl"
        >
          <Transition name="fade" mode="out-in">
            <TheImage v-if="imageToggle" :src="image" />
            <TheImage v-else :src="image2" />
          </Transition>
        </div>
        <canvas ref="imageCanvas" class="hidden"></canvas>
      </div>
      <div class="mt-4 text-center text-sm text-neutral-400">
        <p>
          <a
            class="underline underline-offset-2"
            href="https://peach.blender.org/"
            target="_blank"
            >Big Buck Bunny</a
          >
          is licensed under the
          <a
            class="underline underline-offset-2"
            href="https://creativecommons.org/licenses/by/3.0/"
            target="_blank"
            >Creative Commons Attribution 3.0 License</a
          >
        </p>
        <p>
          &copy; Copyright 2008
          <a
            class="underline underline-offset-2"
            href="https://www.blender.org/about/foundation/"
            target="_blank"
            >Blender Foundation</a
          >
          /
          <a
            class="underline underline-offset-2"
            href="https://peach.blender.org/"
            target="_blank"
            >www.bigbuckbunny.org</a
          >
        </p>
        <p>
          Clip published on
          <a
            class="underline underline-offset-2"
            href="https://www.npmjs.com/package/big-buck-bunny-1080p"
            target="_blank"
            >npm</a
          >
          by
          <a
            class="underline underline-offset-2"
            href="https://github.com/covertbert/big-buck-bunny-1080p"
            target="_blank"
            >covertbert</a
          >
        </p>
      </div>
    </div>
  </div>
</template>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 6s ease;
}

.fade-enter-from {
  opacity: 0.5;
}
.fade-leave-to {
  opacity: 0.4;
}
</style>
