<script setup>
import { ref } from 'vue'

const shelly = ref('')
const isActive = ref(true)

// function initialState() {
//   fetch(`http://192.168.1.100/relay/0/ison`)
//     .then(response => {
//       // Gérer la réponse de l'API ici
//       isActive.value = response.json()
//     })
//     .catch(error => {
//       // Gérer les erreurs ici
//       console.error('Erreur lors de l\'exécution de l\'action ON :', error);
//     });
// }

// initialState();

// setInterval(initialState, 1000);


function toggle() {
  isActive.value = !isActive.value

  const newState = isActive.value;
  const deviceId = "80646F827174"; // Remplacez par l'ID de votre dispositif.
  const authKey = "MWRmYzM2dWlkE62C6C4C76F817CE0A3D2902F5B5D4C115E49B28CF8539114D9246505DE5D368D560D06020A92480"; // Remplacez par votre clé d'authentification.

  const toggleUrl = `https://shelly-86-eu.shelly.cloud/device/relay/control?channel=0&turn=${newState ? "on" : "off"}&id=${deviceId}&auth_key=${authKey}`;

  fetch(toggleUrl)
  .then(response => {
    // Gérer la réponse de l'API ici
    console.log('Action ON effectuée avec succès');
    // isActive.value = !isActive.value
  })
  .catch(error => {
    // Gérer les erreurs ici
    console.error('Erreur lors de l\'exécution de l\'action ON :', error);
  });
}

</script>

<template>
  <div class="about">
    <div class="container">
      <h1>You can control Shelly connected socket here</h1>
      <button :class="{active:isActive}" @click="toggle()">{{ isActive ? 'ON' : 'OFF' }}</button>
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
    width: 150px;
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
