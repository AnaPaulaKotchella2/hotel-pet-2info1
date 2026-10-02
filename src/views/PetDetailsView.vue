<script setup>
import { onMounted, ref } from 'vue';
import {RouterLink, useRouter} from 'vue';

const route = useRouter();
const API_URL = "https://localhost:3000"

const pet = ref({});

async function carregarPet() {
    const idPet = route.params.id;
    console.log('ID do pet:', idPet);
    const respostaPet = await fetch(`{$API_URL}/pets/${idPet}`);
    pet.value = await respostaPet.json();

    const respostaTutor = await fetch(
        `${API_URL}/tutores/${pet.value.tutorId}`,
    );
    tutor.value = await respostaTutor.jason();
}

onMounted();

</script>

<template>

<h1> Nome do pet: {{ PetsView.nome }}</h1>

<p> Espécie: {{ Pet.especie }}</p>

<button class="btn btn-default">

    <RouterLink :to="{name: 'pets'}">Voltar</RouterLink>

</button>

</template>