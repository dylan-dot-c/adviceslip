<script lang="ts" setup>
import { ref, onMounted } from 'vue'
import type { Ref } from 'vue'
import desktopLine from '../assets/pattern-divider-desktop.svg'
import mobileLine from '../assets/pattern-divider-mobile.svg'
import dice from '../assets/icon-dice.svg'

type Advice = {
  slip_id: number
  advice: string
}

const fetchData = async () => {
  loading.value = true
  const response = await fetch('https://api.adviceslip.com/advice')
  const data = await response.json()
  console.log(data)
  advice.value = { slip_id: data.slip.id, advice: data.slip.advice }
  loading.value = false
}

onMounted(() => {
  fetchData()
})

const advice: Ref<Advice> = ref({ slip_id: 0, advice: '' })
const loading = ref(false)
</script>

<template>
  <div class="container">
    <p class="advice-id">Advice #{{ advice.slip_id }}</p>

    <p class="advice">“{{ advice.advice }}”</p>

    <div class="icon-line">
      <img :src="desktopLine" alt="" class="desktop" />
      <img :src="mobileLine" alt="" class="mobile" />
    </div>

    <div class="dice-container" @click="fetchData">
      <img :src="dice" alt="" width="30px" :class="{ rotate: loading }" />
    </div>
  </div>
</template>

<style scoped>
@import url(../assets/base.css);
.container {
  background-color: var(--dark-gray-blue);
  padding: 1rem;
  border-radius: 1rem;
  text-align: center;
  position: relative;
  width: 480px;
  box-sizing: border-box;
  padding-bottom: 4rem;
}

.advice {
  font-size: 28px;
  color: white;
  /* font-family: var(--manrope); */
  font-weight: bold;
}

.advice-id {
  color: var(--neon-green);
  text-align: center;
  text-transform: uppercase;
  font-size: 12px;
  letter-spacing: 3px;
  font-weight: bold;
}

.dice-container {
  background-color: var(--neon-green);
  width: 30px;
  height: 30px;
  border-radius: 50%;
  padding: 1rem;
  display: grid;
  place-items: center;
  position: absolute;
  bottom: -30px;
  right: 50%;
  transform: translate(50%);

  &:hover {
    box-shadow: 0 0 40px var(--neon-green);
    cursor: pointer;
    animation: pan 3s infinite ease-in;
  }

  img.rotate {
    animation: spin-dice 0.5s infinite linear;
  }
}

.icon-line {
  display: flex;
  justify-content: center;
  align-items: center;
}

.mobile {
  display: none;
}

.desktop {
  display: block;
}
@keyframes pan {
  0%,
  100% {
    box-shadow: 0 0 20px var(--neon-green);
  }
  50% {
    box-shadow: 0 0 40px var(--neon-green);
  }
}

@keyframes spin-dice {
  from {
    transform: rotate(360deg);
  }

  to {
    transform: rotate((0deg));
  }
}

@media screen and (max-width: 600px) {
  .container {
    max-width: 335px;
  }

  .mobile {
    display: block;
  }

  .desktop {
    display: none;
  }
}
</style>
