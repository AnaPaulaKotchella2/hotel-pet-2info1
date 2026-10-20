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

const tutor = ref({});
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

    <p v-if="carregandoPets">Carregando Tutores ...</p>
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

          <input
          type="text"
          id="cpf"
          maxlength="11"
          v-model="novoTutor.cpf"
          class="form-control"
          required
          @input="cpf = cpf.replace(/\D/g, '').slice(0, 11)"
        />
        
      </div>


      

      <div class="col-md-6">
        <label
          for="email"
          class="form-label"
        >
          Email
        </label>

        <input
          type="email"
          id="email"
          v-model="novoTutor.email"
          class="form-control"
          required
        />
      </div>

      <div class="col-md-6">
        <label
          for="telefone"
          class="form-label"
        >
          Telefone
        </label>

        <input
          type="tel"
          id="email"
          v-model="novoTutor.telefone"
          class="form-control"
          required
        />
      </div>
      <div class="col-md-6">
        <label
          for="nascimento"
          class="form-label"
        >
          Data de nascimento
        </label>

        <input
          type="date"
          id="nascimento"
          v-model="novoTutor.nascimento"
          class="form-control"
          required
        />
      </div>
     
      <button
        type="submit"
        class="btn btn-success mt-2"
      >
        Salvar Tutor
      </button>
    </form>
  </div>
</template>