<script setup lang="ts">
const cursos = ['Ciência da Computação', 'Sistemas de Informação', 'Engenharia de Software', 'ADS', 'Outro']
const opcoesInteresses = ['Front-end', 'Back-end', 'Mobile', 'UI/UX', 'Dados/IA', 'DevOps']
const MAX_BIO = 200

const form = reactive({
  nome: '',
  email: '',
  curso: '',
  semestre: null as number | null,
  interesses: [] as string[],
  bio: '',
})

const erros = reactive<Record<string, string>>({})
const sucesso = ref(false)
const enviado = ref<typeof form | null>(null)

function validar() {
  Object.keys(erros).forEach((k) => delete erros[k])

  if (!form.nome.trim()) erros.nome = 'O nome é obrigatório.'
  if (!form.email.trim()) erros.email = 'O e-mail é obrigatório.'
  else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(form.email)) erros.email = 'Formato de e-mail inválido.'
  if (!form.curso) erros.curso = 'Selecione um curso.'
  if (!form.semestre || form.semestre < 1 || form.semestre > 12) erros.semestre = 'Informe um semestre entre 1 e 12.'
  if (form.interesses.length === 0) erros.interesses = 'Escolha ao menos um interesse.'
  if (form.bio.length > MAX_BIO) erros.bio = `Máximo de ${MAX_BIO} caracteres.`

  return Object.keys(erros).length === 0
}

function enviar() {
  sucesso.value = false
  if (!validar()) return

  enviado.value = JSON.parse(JSON.stringify(form))
  console.log('Novo membro cadastrado:', enviado.value)
  sucesso.value = true

  // limpa os campos
  form.nome = ''
  form.email = ''
  form.curso = ''
  form.semestre = null
  form.interesses = []
  form.bio = ''

  setTimeout(() => (sucesso.value = false), 5000)
}
</script>

<template>
  <main class="mx-auto max-w-2xl p-4 sm:p-8">
    <div class="card bg-base-100 shadow-md">
      <form class="card-body gap-4" novalidate @submit.prevent="enviar">
        <h1 class="card-title text-2xl">Cadastro de novo membro</h1>

        <div v-if="sucesso" role="alert" class="alert alert-success">
          <span>Cadastro de <strong>{{ enviado?.nome }}</strong> realizado com sucesso!</span>
        </div>

        <!-- Nome -->
        <label class="form-control w-full">
          <span class="label-text mb-1">Nome completo</span>
          <input v-model="form.nome" type="text" placeholder="Seu nome"
            class="input input-bordered w-full" :class="{ 'input-error': erros.nome }" />
          <span v-if="erros.nome" class="mt-1 text-sm text-error">{{ erros.nome }}</span>
        </label>

        <!-- E-mail -->
        <label class="form-control w-full">
          <span class="label-text mb-1">E-mail</span>
          <input v-model="form.email" type="email" placeholder="voce@email.com"
            class="input input-bordered w-full" :class="{ 'input-error': erros.email }" />
          <span v-if="erros.email" class="mt-1 text-sm text-error">{{ erros.email }}</span>
        </label>

        <div class="grid grid-cols-1 gap-4 sm:grid-cols-2">
          <!-- Curso -->
          <label class="form-control w-full">
            <span class="label-text mb-1">Curso / Área</span>
            <select v-model="form.curso" class="select select-bordered w-full"
              :class="{ 'select-error': erros.curso }">
              <option disabled value="">Selecione...</option>
              <option v-for="c in cursos" :key="c" :value="c">{{ c }}</option>
            </select>
            <span v-if="erros.curso" class="mt-1 text-sm text-error">{{ erros.curso }}</span>
          </label>

          <!-- Semestre -->
          <label class="form-control w-full">
            <span class="label-text mb-1">Semestre</span>
            <input v-model.number="form.semestre" type="number" min="1" max="12"
              class="input input-bordered w-full" :class="{ 'input-error': erros.semestre }" />
            <span v-if="erros.semestre" class="mt-1 text-sm text-error">{{ erros.semestre }}</span>
          </label>
        </div>

        <!-- Interesses -->
        <fieldset>
          <legend class="label-text mb-2">Interesses / Habilidades</legend>
          <div class="grid grid-cols-2 gap-2 sm:grid-cols-3">
            <label v-for="i in opcoesInteresses" :key="i" class="label cursor-pointer justify-start gap-2">
              <input v-model="form.interesses" type="checkbox" :value="i" class="checkbox checkbox-primary checkbox-sm" />
              <span class="label-text">{{ i }}</span>
            </label>
          </div>
          <span v-if="erros.interesses" class="text-sm text-error">{{ erros.interesses }}</span>
        </fieldset>

        <!-- Bio -->
        <label class="form-control w-full">
          <span class="label-text mb-1">Mensagem / Bio curta</span>
          <textarea v-model="form.bio" :maxlength="MAX_BIO" rows="4"
            class="textarea textarea-bordered w-full" placeholder="Fale um pouco sobre você..." />
          <span class="mt-1 text-right text-xs opacity-70">{{ form.bio.length }}/{{ MAX_BIO }}</span>
          <span v-if="erros.bio" class="text-sm text-error">{{ erros.bio }}</span>
        </label>

        <button type="submit" class="btn btn-primary">Cadastrar</button>
      </form>
    </div>
  </main>
</template>