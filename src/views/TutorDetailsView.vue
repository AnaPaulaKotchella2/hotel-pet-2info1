<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRoute } from 'vue-router';

const route = useRoute();
const API_URL = 'http://localhost:3000';
// exibir informações do pet e do tutor
const pet = ref({});
const tutor = ref({});

async function carregarTutor() {
  // exibindo os dados do Id do Pet
  const idTutor = route.params.id;
  console.log('ID do Tutor:', idTutor);

  const respostaTutor = await fetch(`${API_URL}/tutores/${idTutor}`);
  tutor.value = await respostaTutor.json();

}

onMounted(carregarTutor);
</script>

<template>
  <h1>Nome do Tutor: {{ tutor.nome }}</h1>
  <p>Telefone tutor: {{ tutor.telefone }}</p>
  <p>Cpf Tutor:  {{ tutor.cpf }}</p>
  <p>Email Tutor: {{ tutor.email }}</p>
  <p>DataNascimento Tutor: {{ tutor.nascimento }}</p>
  <p>Nome do Pet: {{ pet?.nome }}</p>
  

  <button class="btn btn-default">
    <RouterLink :to="{ name: 'tutores' }"> Voltar </RouterLink>
  </button>
</template>