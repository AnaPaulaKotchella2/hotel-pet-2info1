<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';

const API_URL = 'http://localhost:3000';

const pets = ref([]);
const tutores = ref([]);
const loading = ref(true);

async function carregarDados() {
  const respostaPets = await fetch(`${API_URL}/tutores`);
  pets.value = await respostaPets.json();

  console.log('Pets:', pets.value);

  const respostaTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostaTutores.json();

  console.log('Tutores:', tutores.value);

  loading.value = false;
}

function nomeDoTutor(tutorid) {
  for (const tutor of tutores.value) {
    console.log('tutor', tutorid);
    // tutor.id == tutorId
    if (tutor.id === tutorid) {
      return tutor.nome;
    }
  }
  return 'Tutor Não Encontrado!';
};

async function excluirTutor(id) {
  const confirmarExclusao = window.confirm('Deseja realmente excluir o Tutor?');

  if (confirmarExclusao) {
    // Lógica para excluir o pet
    await fetch(`${API_URL}/Tutor/${id}`, {
      method: 'DELETE',
    });
    // Recarrega os dados após a exclusão
    await carregarDados();
  }
}

// Recebe o tutorId do pet e procura o tutor correspondente.
onMounted(carregarDados);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Pets cadastrados no sistema.
      </p>
    </header>

    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addTutor' }"
    >
      Adicionar Tutor
    </RouterLink>

    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Especie</th>
          <th>Tutor</th>
          <th>Ações</th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="tutor in tutores"
          :key="tutor.id"
        >
          <td>{{ tutor.id }}</td>
          <td>{{ tutor.nome }}</td>
          <td>{{ tutor.telefone }}</td>
          <td>{{ nomeDoTutor(tutor.tutoresId) }}</td>
          <td>
            <RouterLink :to="`/tutores/${tutores.id}`"> Visualizar </RouterLink>
            <button
              class="btn btn-danger mr-5"
              @click="excluirTutor(tutores.id)"
            >
              Excluir
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>