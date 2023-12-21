<script setup>
import { ref, onMounted, toRefs } from "vue";
import { Head, Link, router } from "@inertiajs/vue3";

import MainLayout from "@/Layouts/MainLayout.vue";
import ShowPostOverlay from "@/Components/ShowPostOverlay.vue";

import "vue3-carousel/dist/carousel.css";
import { Carousel, Slide, Navigation } from "vue3-carousel";

import DotsHorizontal from "vue-material-design-icons/DotsHorizontal.vue";

let wWidth = ref(window.innerWidth);
let currentSlide = ref(0);
let currentPost = ref(null);
let openOverlay = ref(true);

const addComment = () => {
  console.log("ADD");
};
const updateLike = () => {
  console.log("Update");
};
const deleteFunc = () => {
  console.log("Delete");
};

onMounted(() => {
  window.addEventListener("resize", () => {
    wWidth.value = window.innerWidth;
  });
});
</script>
<template>
  <Head title="Instagram" />

  <MainLayout>
    <div class="mx-auto lg:pl-0 md:pl-[80px] pl-0">
      <Carousel
        v-model="currentSlide"
        class="max-w-[700px] mx-auto"
        :items-to-show="wWidth >= 768 ? 8 : 6"
        :items-to-scroll="4"
        :wrap-around="true"
        :transition="500"
        snapAlign="start"
      >
        <Slide v-for="slide in 10" :key="slide">
          <Link
            href="/"
            class="relative mx-auto text-center mt-4 px-2 cursor-pointer"
          >
            <div
              class="absolute z-[-1] -top-[5px] left-[4px] rounded-full rotate-45 w-[64px] h-[64px] contrast-[1.3] bg-gradient-to-t from-yellow-300 to-purple-500 via-red-500"
            >
              <div
                class="rounded-full ml-[3px] mt-[3px] w-[58px] h-[58px] bg-white"
              />
            </div>
            <img
              class="rounded-full w-[56px] h-[56px] -mt-[1px]"
              src="/user-placeholder.png"
            />
            <div
              class="text-xs mt-2 w-[60px] truncate text-ellipsis overflow-hidden"
            >
              <!-- {{ slide.name }} -->
            </div>
          </Link>
        </Slide>

        <template #addons>
          <Navigation />
        </template>
      </Carousel>

      <div
        id="Posts"
        class="px-4 max-w-[600px] mx-auto mt-10"
        v-for="post in 1"
        :key="post"
      >
        <div class="flex items-center justify-between py-2">
          <div class="flex items-center">
            <Link href="/" class="flex items-center">
              <img class="rounded-full w-[38px] h-[38px]" src="/19.jpg" />
              <div class="ml-4 font-extrabold text-[15px]">
                This wo has the post
              </div>
            </Link>
            <div class="flex items-center text-[15px] text-gray-500">
              <span class="-mt-5 ml-2 mr-[5px] text-[35px]">.</span>
              <div>21 Dec 2023</div>
            </div>
          </div>

          <DotsHorizontal class="cursor-pointer" :size="27" />
        </div>

        <div class="bg-black rounded-lg w-full min-h-[400px] flex items-center">
          <img class="mx-auto w-full" src="/boho.png.jpg" />
        </div>

        <LikesSection :post="post" @like="updateLike($event)" />

        <div class="text-black font-extrabold py-1">20 likes</div>
        <div>
          <span class="text-black font-extrabold"> This wo has the post </span>
          Caption Here
        </div>
        <button
          @click="
            currentPost = post;
            openOverlay = true;
          "
          class="text-gray-500 font-extrabold py-1"
        >
          View all 5 comments
        </button>
      </div>

      <div class="pb-20"></div>
    </div>
  </MainLayout>

  <ShowPostOverlay
    v-if="openOverlay"
    :post="currentPost"
    @addComment="addComment($event)"
    @updateLike="updateLike($event)"
    @deleteSelected="deleteFunc($event)"
    @closeOverlay="openOverlay = false"
  />
</template>

<style>
.carousel__prev,
.carousel__next,
.carousel__prev:hover,
.carousel__next:hover {
  color: rgb(49, 49, 49);
  background-color: rgb(255, 255, 255);
  border-radius: 100%;
  margin: 0 20px;
}
</style>
