<script setup>
import HelloWorld from './components/HelloWorld.vue'
import { ref, onMounted } from 'vue';
import PocketBase from 'pocketbase';


let count = ref(0);
let limit = ref(false);
let prayerTime = ref({});
let pokemon = ref([]);
let records = ref([]);
let buatapa = ref("");
let dahke = ref(false);
let sudah = ref("");
const pb = new PocketBase('http://127.0.0.1:8090');

async function getTodo(){
  records.value = await pb.collection('todos').getFullList();
  console.log(records.value)

}
if(sudah.value=="dah"){
  dahke.value=true
}
else{
  dahke.value=false
}
async function setTodo(){
  const data = {
    "item": buatapa.value,
    "isDone": dahke
};

const record = await pb.collection('todos').create(data);
}


const solatApiLink = "https://www.e-solat.gov.my/index.php?r=esolatApi/takwimsolat&period=today&zone=JHR02";
const pokeApiLink = "https://pokeapi.co/api/v2/pokemon"
function addNumber(){
  if(count.value<10){
    count.value++;
    limit.value=false;
  }else{
    limit.value=true;
  }
}
function getPokemon(){
  fetch(pokeApiLink).then(result=>result.json()).then(data=>{
    pokemon.value = data.results
    console.log(pokemon.value)
  })
}
function getPrayerTime(){
  fetch(solatApiLink).then(result => result.json()).then(data=>{
    prayerTime.value = data.prayerTime[0]
    console.log(prayerTime.value)
  }
    
  )
}
onMounted(()=>{
  getPrayerTime()
  getTodo()
  })
</script>

<template>
  <input type="text" v-model="buatapa">
  <input type="text" v-model="sudah">
 
 <h1>tekan butang dibawah</h1>
 <button @click="setTodo" v-if="limit==false">tekanlah</button>
 <p v-if="limit==false"> {{ count }}</p>
 <p v-if="limit==true">Dahlah!! Sampai 10 je lebih lebih lak</p>
 <button @click="getPrayerTime">test</button>
 <h1>maghrib time:</h1>
 <h2>{{ prayerTime.maghrib }}</h2>
<div class="'border rounded-sm bg-slate-300 m-2 shadow-md p-2">
 <table class="w-full">
  <thead>
    <tr>
      <th>Fajr</th>
      <th>Dhuha</th>
      <th>Dhuhr</th>
      <th>Asr</th>
      <th>Maghrib</th>
      <th>Isyak</th>
    </tr>
  </thead>
  <tbody>
    <tr class="text-senter">
      <td>{{ prayerTime.fajr }}</td>
      <td>{{ prayerTime.dhuha }}</td>
      <td>{{ prayerTime.dhuhr }}</td>
      <td>{{ prayerTime.asr }}</td>
      <td>{{ prayerTime.maghrib }}</td>
      <td>{{ prayerTime.isha }}</td>
    </tr>
  </tbody>
 </table>
 </div>

 <button @click="getPokemon">pokemon</button>
 <ol>
  <li v-for="poke in pokemon" >
    {{ poke.name }}
  </li>
 </ol>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>



