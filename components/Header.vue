<template>
  <div
    class="
      container
      mx-auto
      flex flex-wrap
      p-5
      flex-col
      md:flex-row
      items-center
      bg-black
      border-b-2
      border-white
      border-opacity-80
    "
  >
    <nav class="flex lg:w-2/5 flex-wrap items-center text-base md:ml-auto">
      <NuxtLink
        to="/fitness"
        class="
          mr-5
          cursor-pointer
          border-b border-transparent
          text-gray-600
          hover:text-white
          uppercase
        "
        >Fitness</NuxtLink
      >
      <NuxtLink
        to="/bowling"
        class="
          mr-5
          cursor-pointer
          border-b border-transparent
          text-gray-600
          hover:text-white
          uppercase
        "
        >Bowling</NuxtLink
      >
      <NuxtLink
        to="/analytics"
        class="
          mr-5
          cursor-pointer
          border-b border-transparent
          text-gray-600
          hover:text-white
          uppercase
        "
        >Analytics</NuxtLink
      >
    </nav>
    <div
      class="
        flex
        order-first
        lg:order-none
        lg:w-1/5
        title-font
        font-medium
        items-center
        lg:items-center
        lg:justify-center
        mb-4
        md:mb-0
      "
    >
      <img
        src="~assets/logo.png"
        style="height: 40px; margin-top: 10px; margin-bottom: 10px"
        alt="logo"
      />
      <NuxtLink to="/" class="ml-3 text-xl text-white">Sport Diary</NuxtLink>
    </div>
    <div class="lg:w-2/5 inline-flex lg:justify-end ml-5 lg:ml-0">
      <Button title="Sign up" @onClickButton="onClickSignUp"></Button>
    </div>
  </div>
</template>
<script>
export default {
  methods: {
    onClickSignUp () {
      alert('회원가입을 넣을까 말까');
    }
  }
}
</script>