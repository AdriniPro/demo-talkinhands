<script setup>

import {reactive , ref, onMounted} from 'vue'
import { useRouter } from 'vue-router'
import axios from "axios";

const router = useRouter()

let form = reactive({
    name:'',
    username:'',
    email:'',
    password:'',
    c_password:''
})

let error = ref('')
let id = ref()

onMounted(async() => {
    userData()
})


const userData = async() =>{
    await axios.get('/api/user-profile')
        .then(response => {
            if(response.data.success){
                id.value = response.data.data._id
            }else{
                error.value = response.data.message;
            }
        })
}

const update = async() =>{
    await axios.put('/api/users/'+ id.value, form)
        .then(response => {
            if(response.data.success){
                router.push('/home')
            }else{
                error.value = response.data.message;
            }
        })
}

</script>


<template>
    <div>
        <h1>Actualiza tus datos</h1>
    </div>

    <br>

    <div>
        <p v-if="error">{{ error }}</p>
        <form @submit.prevent="update">
            <input type="text" placeholder="Enter your name" v-model="form.name">
            <br>
            <input type="text" placeholder="Enter your usernam" v-model="form.username">
            <br>
            <input type="email" placeholder="Enter your email" v-model="form.email">
            <br>
            <input type="password" placeholder="Enter your password" v-model="form.password">
            <br>
            <input type="password" placeholder="Confirm your password" v-model="form.c_password">
            <br>
            <input type="submit" value="Actualizar">
        </form>
    </div>
</template>

