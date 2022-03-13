<script setup>
import ButtonRepo from "@/components/ButtonRepo.vue";
import { ref } from "vue";
import axios from "axios";
axios.defaults.baseURL = "http://127.0.0.1:8000/api/";
let isVoted = ref(true);
let isLoading = ref(true);
let isLoadingOverlay = ref(false);
let Result = ref([]);
let rateTitle = {
  1: "Good",
  2: "Fair",
  3: "Neutral",
  4: "Bad",
};
async function Vote(value) {
  try {
    isLoading.value = false;
    isLoadingOverlay.value = true;
    const response = await axios
      .post("rating", { vote: value })
      .then((res) => {
        if (res.data.status == "success") {
          axios.get("getResults", { vote: value }).then((result) => {
            Result.value = result.data.data;
            isLoadingOverlay.value = false;
          });
        }
      })
      .catch(() => {});
  } catch (error) {
    console.error(error);
  }
}
</script>

<template>
  <div class="bg-gray-50">
    <div v-if="isLoading">
      <div
        class="m-4 px-4 py*t-6 font-extrabold leading-9 tracking-tight text-gray-900 sm:text-2xl sm:leading-10"
      >
        How do you find our service?
      </div>
      <div
        class="mx-auto max-w-screen-xl sm:px-6 lg:flex lg:items-center lg:justify-between lg:px-8"
      >
        <div class="mt-2 flex lg:mt-0 lg:flex-shrink-0">
          <ButtonRepo
            @vote="Vote"
            :isLoading="isLoading"
            title="Good"
            value="1"
            hoverBg='hover:bg-green-400'
          />
          <ButtonRepo
            @vote="Vote"
            :isLoading="isLoading"
            title="Fair"
            value="2"
              hoverBg='hover:bg-green-200'
          />
          <ButtonRepo
            @vote="Vote"
            :isLoading="isLoading"
            title="Neutral"
            value="3"
               hoverBg='hover:bg-yellow-200'
          />
          <ButtonRepo
            @vote="Vote"
            :isLoading="isLoading"
            title="Bad"
            value="4"
               hoverBg='hover:bg-red-200'
          />
        </div>
      </div>
    </div>
    <div v-else>
      <button
        @click="isLoading = true"
        class="tracking-tight bg-red-300 px-6 text-gray-900 sm:text-2xl m-3 sm:leading-10"
      >
        Back
      </button>
      <div
        class="m-4 px-4 py*t-6 font-extrabold text-center leading-9 tracking-tight text-gray-900 sm:text-2xl sm:leading-10"
      >
        Results

        <br />
        <div v-for="resVote in Result" :key="resVote">
          <div>
            {{
              rateTitle[resVote.vote] +
              " " +
              resVote.percent +
              "%" +
              " - " +
              resVote.no_of_voters +
              " results"
            }}
          </div>
          <div></div>
        </div>
      </div>
    </div>
  </div>
  <div
    v-show="isLoadingOverlay"
    wire:loading
    class="fixed top-0 left-0 right-0 bottom-0 w-full h-screen z-50 overflow-hidden bg-gray-700 opacity-75 flex flex-col items-center justify-center"
  >
    <div
      class="loader ease-linear rounded-full border-4 border-t-4 border-gray-200 h-12 w-12 mb-4"
    ></div>
  </div>
</template>
<style scoped>
.loader {
  border-top-color: #3498db;
  -webkit-animation: spinner 1.5s linear infinite;
  animation: spinner 1.5s linear infinite;
}

@-webkit-keyframes spinner {
  0% {
    -webkit-transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
  }
}

@keyframes spinner {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
