<script setup>
import { onMounted, ref } from 'vue';
import {RouterLink, useRouter} from 'vue';

const pet = ref({});
const tutor = ref([]);

const route = useRouter();
const API_URL = "https://localhost:3000"


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

onMounted(carregarPet);

</script>

<template>

<h1> Nome do pet: {{ pet.nome }}</h1>

<p> Espécie: {{ pet.especie }}</p>

<p> Tutor: {{ tutores.find(tutor => tutor.id === pet.tutorId)?.nome }}</p>

<button class="btn btn-primary">

    
</button>

</template>