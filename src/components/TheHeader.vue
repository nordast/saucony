<script setup>
const emit = defineEmits(['openDrawer'])

defineProps({
  totalPrice: Number,
})
</script>

<template>
  <div class="bg-[#ff0000] text-white border-b border-slate-200 p-1 text-center">
    <span class="capitalize italic text-xl font-black">Holiday Starts Now</span>
    <span class="font-semibold">
      &nbsp; 25% Off<sup>†</sup>
      &nbsp;With Code
      <strong class="underline">CHEER</strong>
    </span>
    &nbsp;&nbsp;
    <span class="text-sm">
      <sup>†</sup>
      Some exclusions apply.
    </span>
  </div>

  <header class="flex justify-between border-b border-slate-200 px-10 py-10">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.svg" alt="Logo" class="w-40" />
      </div>
    </router-link>

    <ul class="flex items-center gap-10">
      <router-link to="/favorites">
        <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
          <img src="/heart.svg" alt="Favorite" />
          <span>Wishlist</span>
        </li>
      </router-link>

      <li
        @click="() => emit('openDrawer')"
        class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black"
      >
        <img src="/cart.svg" alt="Cart" />
        <span>${{ totalPrice }}.00</span>
      </li>

      <!--      <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">-->
      <!--        <img src="/profile.svg" alt="Profile" />-->
      <!--        <span>Profile</span>-->
      <!--      </li>-->
    </ul>
  </header>

  <div
    class="text-white bg-slate-100 bg-[url('/images/community.jpg')] bg-no-repeat bg-cover bg-center border-b border-slate-200 py-20 text-center space-y-8"
  >
    <h1 class="text-5xl font-mono font-black italic">'Tis the Season</h1>
    <p class="text-2xl font-bold">
      For you and your crew, here's to checking <br />
      everyone (including yourself) off the list.
    </p>
  </div>
</template>
