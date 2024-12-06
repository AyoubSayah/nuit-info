<template>
    <div class="sniper-container  bg-slate-800" @mousemove="updateCursor" @mouseleave="hideCursor" @click="fireShot">
        <div class="relative w-full h-full">
            <img v-show="cursorVisible" src="../../assets/sniper.png" alt="sniper scope" class="sniper-scope"
                :style="{ top: cursorY + 'px', left: cursorX + 'px' }" />
            <ShootingRange />

            <div v-if="showShot" :style="{ left: cursorX + 70 + 'px', top: cursorY + 70 + 'px' }" class="shot">
            </div>

            <DotLottieVue class=" h-full w-full   object-cover " autoplay loop :src="sea" />

            <DotLottieVue v-for="(y, index) in test "
                :style="{ top: y.top, right: y.left, animationDelay: y.delay, animationDuration: y.duration }"
                class=" h-[190px] w-[190px] box z-40  object-cover " autoplay loop :src="salhoufa"
                @click="handleClick(index)" />
            <DotLottieVue v-for="(y, index) in killerPeople "
                :style="{ top: y.top, right: y.left, animationDelay: y.delay, animationDuration: y.duration }"
                class=" h-[190px] w-[190px] box z-40  object-cover " autoplay loop :src="ghawes"
                @click="handlekillHuman(index)" />
                <img 
                :style="{ top:'300px' , right: 0, animationDelay: 30, animationDuration: 2 }"
                class=" h-[190px] w-[190px] box z-40  object-cover " :src="logo"
                @click="handleClickLogo()" />
            <div class="flex flex-col text-8xl text-center ml-4 text-white absolute top-32 ">
                <div>
                    Score
                </div>
                <div :class="{ 'text-green-500': score > 0, 'text-red-500': score < 0 }">
                    {{ score }}
                </div>
            </div>
        </div>
    </div>
</template>
<script setup>

import { debounce } from '@/utils/utils'
import { DotLottieVue } from '@lottiefiles/dotlottie-vue'
import sea from '@/assets/sea.lottie'
import salhoufa from '@/assets/salhoufa.lottie'
import ghawes from '@/assets/ghawes.lottie'
import logo from '@/assets/logo.png'

import { ref, watch, defineProps } from 'vue'
const props = defineProps(['closeGaming'])
const closeGaming = props.closeGaming
const generateRandomPositions = (numPositions) => {
    const positions = [];

    for (let i = 0; i < numPositions; i++) {
        const randomTop = `${Math.random() * 90}%`; // Random top position (percentage)
        const randomLeft = Math.random() < 0.5 ? '0' : '100%'; // Random left: 0 or 100%
        const delay = `${Math.random() * 2}s`; // Random delay between 0s to 2s
        const duration = `${Math.random() * 10 + 3}s`; // Random delay between 0s to 2s

        positions.push({
            top: randomTop,
            left: randomLeft,
            delay: delay,
            duration: duration// Adding delay property
        });
    }

    return positions;
}
const test = ref(generateRandomPositions(4))
const killerPeople = ref(generateRandomPositions(4))

const score = ref(0)

const handleClick = (index) => {
    // Remove the element at the clicked index
    score.value = score.value - 1
    test.value.splice(index, 1);
    if (test.value.length === 0) {
        closeGaming(-1)
    }
}
const handleClickLogo = () => {
       closeGaming(9999)

}
const handlekillHuman = (index) => {
    // Remove the element at the clicked index
    score.value = score.value + 1
    killerPeople.value.splice(index, 1);
    if (killerPeople.value.length === 0) {
        closeGaming(1)
    }
}

</script>

<script>


export default {
    data() {
        return {
            cursorX: 0,
            cursorY: 0,
            showShot: false,
            cursorVisible: false,
        };
    },
    methods: {
        updateCursor(event) {
            this.cursorX = event.clientX - 50; // Adjust to center the image
            this.cursorY = event.clientY - 50;
            this.cursorVisible = true;
        },
        hideCursor() {
            this.cursorVisible = false;
        },
        fireShot(event) {
            const rect = event.currentTarget.getBoundingClientRect();
            this.shotX = event.clientX - rect.left;
            this.shotY = event.clientY - rect.top;
            debounce(() => {


                this.showShot = true;
                setTimeout(() => {
                    this.showShot = false;
                }, 300);
            }, 300)()

        },

    },
};
</script>

<style scoped>
.sniper-container {
    cursor: none;
    /* Hide the default cursor */
    height: 100vh;
    position: fixed;
    width: 100vw;
    overflow: hidden;

    z-index: 30;
}

.sniper-scope {
    position: absolute;
    width: 150px;
    /* Adjust to match your image size */
    height: 150px;
    pointer-events: none;
    /* Prevent scope from blocking interactions */
}

.shot {
    position: absolute;
    width: 10px;
    height: 10px;
    background: red;
    border-radius: 50%;
    animation: shotEffect 0.3s ease-out;
}

.fire-btn {
    position: absolute;
    bottom: 10px;
    left: 50%;
    transform: translateX(-50%);
    padding: 10px 20px;
    font-size: 16px;
}

@keyframes shotEffect {
    0% {
        transform: scale(1);
        opacity: 1;
    }

    100% {
        transform: scale(3);
        opacity: 0;
    }
}

.box {
    position: absolute;

    animation: moveLeftToEnd 5s linear infinite;
    /* Adjust duration and iteration */
}

@keyframes moveLeftToEnd {
    0% {
        right: 0;
    }

    100% {
        right: 100%;
    }
}
</style>