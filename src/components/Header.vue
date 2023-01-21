<template>
  <div class="w-full max-w-3xl mx-auto flex items-center justify-between">
    <img :src="logoImage" alt="" />

    <button
      @click="buttonClicked"
      type="button"
      class="border border-violet-500 font-semibold rounded-lg px-6 py-4 flex items-center gap-3 hover:border-violet-300 transition-colors focus:outline-none focus:ring-2 focus:ring-violet-600 focus:ring-offset-2 focus:ring-offset-background"
    >
      <PhPlus size="20" class="text-violet-500" />
      Novo hábito
    </button>
  </div>
  <Dialog :open="isModalOpen" @close="setModalIsOpen">
    <div class="w-screen h-screen bg-black/80 fixed inset-0">
      <DialogPanel
        class="absolute p-10 bg-zinc-900 rounded-2xl w-full max-w-md top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"
      >
        <DialogTitle class="text-3xl leading-tight font-extrabold"
          >Criar hábito</DialogTitle
        >
        <NewHabitForm />
      </DialogPanel>
    </div>
  </Dialog>
</template>

<script setup lang="ts">
import logoImage from "@/assets/logo.svg";
import NewHabitForm from "@/components/NewHabitForm.vue";
import { Dialog, DialogPanel, DialogTitle } from "@headlessui/vue";
import { PhPlus } from "phosphor-vue";
import { ref } from "vue";
const isModalOpen = ref(false);

function buttonClicked() {
  isModalOpen.value = true;
}

function setModalIsOpen(value: any) {
  isModalOpen.value = value;
}
</script>
