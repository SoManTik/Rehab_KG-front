<script setup>
import { ref, onMounted, computed, watch, nextTick } from 'vue'
import { RouterLink,useRoute, useLink ,useRouter} from 'vue-router'

const router = useRouter();
const route = useRoute();

var booking = ref('')


const getBooking = async function () {

const response = await fetch('/api'); 
const json = await response.json();

console.log(json);

booking.value = json;
}

onMounted(async () => {

        getBooking();
});
</script>

<template>
  <main>
   {{ booking }}
   
  </main>
</template>
