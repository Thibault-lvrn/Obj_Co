<script setup>
import axios from 'axios';

const id = '083a8dc147d9';
const authKey = 'MWYwM2Q2dWlkBBBFAC4F49BFB7094B9150C9F212BE1554B887BFEB4C943FACAE317F921C1089805C1285B9DEB2A9';

let data = {};
let state = false;

const fetchData = async () => {
  try {
    const response = await axios.get(`https://shelly-86-eu.shelly.cloud/device/status?id=${id}&auth_key=${authKey}`);
    data = response.data.data.device_status;
    state = data.relays[0].ison;
  } catch (error) {
    console.error(error);
  }
};

const toggle = () => {
  const formData = new FormData();
  formData.append('id', id);
  formData.append('turn', state ? 'off' : 'on');
  formData.append('auth_key', authKey);
  formData.append('channel', '0');

  const headers = formData.getHeaders ? formData.getHeaders() : { 'Content-Type': 'multipart/form-data' };
  const config = {
    method: 'post',
    maxBodyLength: Infinity,
    url: 'https://shelly-86-eu.shelly.cloud/device/relay/control',
    headers: {
      ...headers,
    },
    data: formData,
  };

  axios
    .request(config)
    .then((response) => {
      console.log(JSON.stringify(response.data));
    })
    .catch((error) => {
      console.log(error);
    });

  state = !state;
};

fetchData();
</script>

<template>
  <div class="about">
    <div class="container">
      <h1>You can control Shelly connected socket here</h1>
      <button :class="{ active: state }" @click="toggle">{{ state ? 'ON' : 'OFF' }}</button>
      <ul class="informations">
        <li>Too hot ? : {{ data.overtemperature ? 'Oui' : 'Non' }}</li>
        <li>Temperature : {{ data.temperature }}</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
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
