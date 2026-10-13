<script setup>
import { onMounted, ref } from 'vue';

// estava escrito como useRouter e o correto é UseRoute, pois não estamos navegando para outra rota, apenas pegando o parâmetro da rota atual
import { RouterLink, useRoute } from 'vue-router';

const pet = ref({});
const tutor = ref([]);

const route = useRoute();
// aqui estamos pegando o id do pet que está na rota atual, que foi passado como parâmetro na rota estava usando https e o correto era http, pois o json-server não suporta https
const API_URL = 'http://localhost:3000';

async function carregarPet() {
  // aqui estamos pegando o id do pet que está na rota atual, que foi passado como parâmetro na rota
  const idPet = route.params.id;
  console.log('ID do pet:', idPet);

  const respostaPet = await fetch(`${API_URL}/pets/${idPet}`);
  pet.value = await respostaPet.json();

  const respostaTutor = await fetch(`${API_URL}/tutores/${pet.value.tutorId}`);
  tutor.value = await respostaTutor.json();
}

onMounted(carregarPet);
</script>

<template>
  <h1>Nome do pet: {{ pet.nome }}</h1>

  <p>Espécie: {{ pet.especie }}</p>

  <p>Tutor: {{ tutor?.nome }}</p>

  <button class="btn btn-primary">
    <RouterLink :to="{ name: 'pets' }">Voltar</RouterLink>
  </button>
</template>
