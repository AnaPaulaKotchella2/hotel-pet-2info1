<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';
const cpf = ref('');

const novoTutor = ref({
  nome: '',
  especie: '',
  tutorId: '',
});

const router = useRouter();

// aqui estou declarando a API que vai retornar a listagem de todos os pets
// será reponsavel por cadastrar novos pets

const API_URL = 'http://localhost:3000';

const tutores = ref({});
const pets = ref({})

async function carregarPets() {
  const resposta = await fetch(`${API_URL}/pets`);
  console.log('Pets carregados:', pets.value);
  pets.value = await resposta.json();
}

async function salvarTutor() {
  await fetch(`${API_URL}/tutores`, {
    method: 'POST',
    header: {
      'Content-type': 'application/json',
    },
    body: JSON.stringify(novoTutor.value),
  });
  router.push('/tutores');
}

onMounted(carregarPets);
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">Cadastro de Pets no sistema.</p>
    </header>

    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addTutor' }"
    >
      Adicionar Tutor
    </RouterLink>

    <p v-if="carregandoPets">Carregando Pets ...</p>
    <div v-else>
      <p
        v-if="erro"
        class="alert alert-danger"
        role="alert"
      >
        {{ erro }}
      </p>
    </div>

    <form @submit.prevent="salvarTutor">
      <div class="col-md-6">
        <label
          for="nome"
          class="form-label"
        >
          Nome do Tutor
        </label>

        <input
          type="text"
          id="nome"
          v-model="novoTutor.nome"
          class="form-control"
          required
        />
      </div>

      <div class="col-md-6">
        <label
          for="cpf"
          class="form-label"
        >
          CPF:
        </label>

        <select
          id="email"
          v-model="novoPet.especie"
          class="form-select"
          required
        >
          <input
          type="number"
          id="nome"
          maxlength="11"
          v-model="novoTutor.nome"
          class="form-control"
          required
          @input="cpf = cpf.replace(/\D/g, '').slice(0, 11)"
        />
        </select>
      </div>

      <div class="col-md-6">
        <label
          for="nome"
          class="form-label"
        >
          Email
        </label>

        <select
          id="email"
          v-model="novoPet.especie"
          class="form-select"
          required
        >
          <input
          type="email"
          id="nome"
          v-model="novoTutor.nome"
          class="form-control"
          required
        />
        </select>
      </div>

      <div class="col-md-6">
        <label
          for="pet"
          class="form-label"
        >
          Pet
        </label>

        <select
          id="tutor"
          v-model="novoTutor.tutorId"
          class="form-select"
          required
        >
          <option
            value=""
            disabled
          >
            Selecione um Tutor
          </option>
          <option
            v-for="tutor in tutores"
            :key="tutor.id"
            :value="tutor.id"
          >
            {{ tutor.nome }}
          </option>
        </select>
      </div>
      <button
        type="submit"
        class="btn btn-success mt-2"
      >
        Salvar Pet
      </button>
    </form>
  </div>
</template>