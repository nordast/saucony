<script setup>
import TheDrawerHead from '@/components/TheDrawerHead.vue'
import CartItemList from '@/components/CartItemList.vue'
import { computed, inject } from 'vue'
import InfoBlock from '@/components/InfoBlock.vue'

const props = defineProps({
  totalPrice: Number,
  isCreatingOrder: Boolean,
})

const emit = defineEmits(['createOrder'])

const buttonDisabled = computed(() => (props.isCreatingOrder ? true : !props.totalPrice))

const { closeDrawer } = inject('cart')

const orderId = null
</script>

<template>
  <div @click="closeDrawer" class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <TheDrawerHead />

    <div v-if="!totalPrice || orderId" class="flex items-center pt-20">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Your cart is empty!"
        description=""
        image-url="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        :title="`Your order #${orderId} has been successfully placed`"
        description="Please expect to be contacted shortly regarding the next steps."
        image-url="/order-success-icon.png"
      />
    </div>

    <div v-else>
      <CartItemList />

      <div class="flex flex-col gap-4 mt-7">
        <div class="flex gap-2">
          <span>Total:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>${{ totalPrice }}</b>
        </div>

        <button
          :disabled="buttonDisabled"
          @click="() => emit('createOrder')"
          class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
        >
          CHECKOUT
        </button>
      </div>
    </div>
  </div>
</template>
