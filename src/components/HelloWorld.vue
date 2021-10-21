<script setup>
import { ref, computed } from 'vue'
import md5 from 'md5'
import syllables from '../assets/syllables'

const name1 = ref('')
const name2 = ref('')

const displayFinalName = ref(false)

function capitalize (string) {
  return string.charAt(0).toUpperCase() + string.slice(1)
}

const hashedName = computed(() => (name1.value && name2.value) && md5(name1.value+name2.value))
const values = computed(() => hashedName.value && hashedName.value.match(/.{8}/g).map(x => parseInt(Number(`0x${x}`), 10)%syllables.length))
const finalName = computed(() => values.value && capitalize(values.value.map(v => syllables[v]).join('').toLowerCase()))

defineProps({
  msg: String
})

</script>

<template>
  <h1>{{ msg }}</h1>

  <form @submit.prevent="displayFinalName = true">
    <div class="formContent">
      <div class="formInput">
        <label for="name1">Entrez le nom du premier conjoint</label>
        <br/>
        <input type="text" id="name1" v-model="name1" />
      </div>
      <div class="formInput">
        <label for="name2">Puis celui du deuxième conjoint</label>
        <br/>
        <input type="text" id="name2" v-model="name2" />
      </div>
    </div>

    <button v-if="!displayFinalName" type="submit">Découvrez votre nom de foyer</button>
  </form>

  <section v-if="displayFinalName">
    <h3>Félicitations ! Votre nom de foyer sera : </h3>
    <h2>{{finalName}}</h2>
  </section>
  
</template>

<style scoped>
a {
  color: #42b983;
}

.formContent {
  display: flex;
  justify-content: space-around;
}

.formInput {
  padding: 48px;
  text-align: left;
  font-size: 24px;
}

input {
  padding: 12px;
  border: none;
  border-bottom: 1px solid #232323;
  background-color: #EFF7CF;
}

button {
  background-color: #42b983;
  color: #202b38;
  border: none;
  padding: 32px 16px;
  border-radius: 16px;
  font-size: 24px;
  cursor: pointer;
}

button:hover {
  opacity: .9;
}
</style>
