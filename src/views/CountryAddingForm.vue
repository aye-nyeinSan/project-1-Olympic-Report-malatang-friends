<script setup lang="ts">
import type { CountryInfo } from '@/types'
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'
import { useMessageStore } from '@/stores/message'
import InputText from '@/components/InputText.vue'
import ImageUpload from '@/components/ImageUpload.vue'
import CountryService from '@/services/CountryService'

const countryInfo = ref<CountryInfo>({
    id: 0,
    countryName: '',
    description: '',
    flag: ''
})



const router = useRouter()
const store = useMessageStore()

function saveCountry() {
    CountryService.saveCountry(countryInfo.value)
    .then((response) => {
      console.log("Response after save country",response.data.id);
      
        router.push({
            name: 'CountryDetails',
            params: { id: response.data.id }
        })
        store.updateMessage('You are successfully add a new country for ' + response.data.countryName)
        setTimeout(() => {
            store.resetMessage()
        }, 3000)
    })
    .catch(() => {
        router.push({ name: 'NetworkError' })
    })
}
</script>

<template>
    <div class="event-form">
        <h1 class="text-2xl text-center">Adding new country</h1><br>
        <form @submit.prevent="saveCountry">
            <h3>Country Name: </h3>
            <InputText v-model="countryInfo.countryName" type="text" label="Country Name" />

            <h3>Description of Country: </h3>
            <InputText v-model="countryInfo.description" type="text" label="Description" />

            <h3>Country Flag</h3>
            <ImageUpload v-model="countryInfo.flag" label="Flag" />

            <button class="button" type="submit">Submit</button>
        </form>
    <pre>{{ countryInfo }}</pre>
    </div>
</template>

<style>
.event-form {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}
h1,
h3 {
  color: #333;
}

form {
  display: flex;
  flex-direction: column;
}

label {
  margin-top: 15px;
  font-weight: bold;
}

.field {
  margin-top: 5px;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #fff;
  width: 100%;
  box-sizing: border-box;
}

.field:focus {
  outline: none;
  border-color: #007bff;
}

.button {
  margin-top: 20px;
  padding: 10px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.button:hover {
  background-color: #0056b3;
}

pre {
  margin-top: 20px;
  background-color: #333;
  color: #fff;
  padding: 10px;
  border-radius: 4px;
  white-space: pre-wrap;
  word-wrap: break-word;
}
</style>