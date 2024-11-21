<template>
  <header class="flex justify-between border-b border-slate-200 px-10 py-8">
    <div class="flex items-center gap-4">
      <img src="/logo.svg" alt="Logo" class="w-40" />
    </div>

    <ul class="flex items-center gap-10">
      <li class="flex items-center gap-3 cursor-pointer">
        <img src="/cart.svg" alt="Cart" />
        <span class="text-gray-500 hover:text-black transition-colors">$0.00</span>
      </li>
      <li class="flex items-center gap-3 cursor-pointer">
        <img src="/heart.svg" alt="Favorite" />
        <span class="text-gray-500 hover:text-black transition-colors">Favorite</span>
      </li>
      <li class="flex items-center gap-3 cursor-pointer">
        <img src="/profile.svg" alt="Profile" />
        <span class="text-gray-500 hover:text-black transition-colors">Profile</span>
      </li>
    </ul>
  </header>
</template>
<script setup lang="ts"></script>
