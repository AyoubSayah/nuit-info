<template>
  <header class="hidden w-full justify-center sm:flex">
    <div
      class="bg-theme-primary text-theme-secondary flex h-28 w-full max-w-[2400px] items-center justify-between py-4 px-16"
      :class="[
        sticky ? 'animate-down-anime fixed left-0  z-30  !h-14 shadow-lg' : '',
      ]">
      <Link to="/" class="w-16">Home</Link>
      <Link to="/articles" class="w-16">Articles</Link>

      <div :class="['h-36 w-[9rem] mt-2', { 'hidden': sticky }]" @click="handleClick">
        <DotLottieVue class="rotate-y cursor-pointer -top-20 relative -ml-[6rem] h-[300px] w-[300px] object-cover "
          autoplay loop :src="salhoufa" />


      </div>

      <Link to="/about" class="w-16">About us</Link>
      <a href="https://incandescent-longma-0374a3.netlify.app/" class="w-16">bad ui</a>

    </div>
  </header>
  <Cursor v-if="isGameStart" :closeGaming="closeGaming" />
  <div v-if="isGameLoading" class="fixed w-full h-full bg-red-500  z-50 flex justify-center items-center">
    <DotLottieVue class=" h-[400px] w-[400px] object-cover " autoplay loop :src="numbers" />

  </div>
  <base-modal :on-close-modal="onCloseModal" :show-modal="showModal">
    <modal-container name="goUp" @click="onCloseModal()" class="h-full w-full flex justify-center items-center">
      <img v-if="picture==9999" alt=" " :src="gameOver" class="w-80 h-80 justify-center items-center" />
      <img alt="" v-if="picture==-1" :src="killAnimals" class="w-80 h-80 justify-center items-center" />

      <img alt=" " v-if="picture==1" :src="kill_human" class="w-80 h-80 justify-center items-center" />

    </modal-container>
  </base-modal>
</template>

<script lang="ts" setup>
import { useThemeStore } from "@/stores/theme";
import { DotLottieVue } from '@lottiefiles/dotlottie-vue'
import Cursor from './cursor.vue'
import useModal from "@/components/base/Modal/useModal";
import BaseModal from "@/components/base/Modal/BaseModal.vue";
import ModalContainer from "@/components/base/Modal/ModalContainer.vue";

import { ref } from 'vue'
import Link from "@/components/Link.vue";
import start from "@/assets/start.mp3"
import shot from "@/assets/shot.mp3"
import { debounce } from '@/utils/utils'
import salhoufa from '@/assets/salhoufa.lottie'

import numbers from '@/assets/numbers.lottie'
import killAnimals from '@/assets/killanimals.jpeg'
import kill_human from '@/assets/kill_human.jpeg'
import gameOver from '@/assets/goodjob.jpg'
import gameovermp3 from '@/assets/gameover.mp3'
import win from '@/assets/good.mp3'

defineProps<{ color?: string; background?: string; sticky?: boolean }>();
const isGameStart = ref(false)
const isGameLoading = ref(false)
const picture = ref(0)
const { showModal, onCloseModal, onOpenModal } = useModal();
const handleClick = async () => {
  await playSound(start)

  isGameLoading.value = true
  setTimeout(() => {
    isGameLoading.value = false
    isGameStart.value = true
    document.removeEventListener("click", playShot)

    document.addEventListener("click", playShot)
  }, 2500);

}
const closeGaming = (value) => {
  isGameStart.value = false
  picture.value=value
  onOpenModal()
  
  if(value==-1 || value ==1){
    playSound(gameovermp3)
  }else{
    playSound(win)

  }
}
const playShot = async () => {

  debounce(() => playSound(shot), 300)()


}
const playSound = async (url) => {
  const audio = new Audio(url);
  await audio.play()

}
const { fullTheme } = useThemeStore();
</script>
