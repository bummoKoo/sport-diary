<template>
  <transition>
    <div
      class="
        flex
        items-center
        justify-center
        fixed
        left-0
        bottom-0
        w-full
        h-full
        bg-gray-800 
        bg-opacity-25
      "
    >
      <div class="bg-white rounded-lg w-1/2">
        <div class="flex flex-col items-start p-4">
          <div class="flex items-center w-full">
            <div class="text-gray-900 font-lg font-bold">
              운동 계획 등록하기
            </div>
            <svg
              class="ml-auto fill-current text-gray-700 w-6 h-6 cursor-pointer"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 18 18"
              @click="onCloseAddFitnessPopup"
            >
              <path
                d="M14.53 4.53l-1.06-1.06L9 7.94 4.53 3.47 3.47 4.53 7.94 9l-4.47 4.47 1.06 1.06L9 10.06l4.47 4.47 1.06-1.06L10.06 9z"
              />
            </svg>
          </div>
          <div class="h-auto">
            <div class="text-black my-3">
              날짜
            </div>
            <range-date-picker></range-date-picker>
            <div class="text-black my-3">
              시간
            </div>
            <div class="text-black my-3">
              운동 종목
            </div>
            <div class="text-black my-3">
              세트 수
            </div>
            <div class="text-black my-3">
              세트 별 쉬는 시간
            </div>
          </div>
          <div class="ml-auto">
            <Button title="등록" @onClickButton="onAddFitnessPlan"></Button>
            <Button
              title="취소"
              @onClickButton="onCloseAddFitnessPopup"
            ></Button>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>
<script>
import RangeDatePicker from './RangeDatePicker.vue';
export default {
  components: { RangeDatePicker },
  methods: {
    onCloseAddFitnessPopup() {
      this.$emit("onCloseAddFitnessPopup");
    },
    onAddFitnessPlan () {
      this.$emit("onAddFitnessPlan");
    }
  },
};
</script>