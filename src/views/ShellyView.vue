<script setup>
// fetch de l'api http://192.168.1.100/shelly :
import { ref } from 'vue'
import { onMounted } from 'vue';

const shelly = ref('')

onMounted(async () => {
  const response = await fetch(`http://192.168.1.100/settings/actions`)
  shelly.value = await response.json()
})

</script>

<template>
  <div class="about">
    <div class="container">
      <h1>You can control Shelly connected socket here</h1>
      <button>On</button>
      <pre>{{ shelly }}</pre>
    </div>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }

  .container {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  button {
    width: 200px;
    margin: 0 auto;
    padding: 10px 30px;
    border-radius: 10px;
    border: 2px solid white;
    background-color: transparent;
    color: white;
    transition: all 0.1s ease-in-out;
  }

  button:hover {
    background-color: white;
    color: black;
  }
}
</style>
