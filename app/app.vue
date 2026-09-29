<template>
  <div class="min-h-screen bg-base-200">
    <div class="navbar bg-base-100 shadow-sm">
      <div class="flex-1">
        <NuxtLink to="/" class="btn btn-ghost text-xl">Meu Projeto</NuxtLink>
      </div>
      <ul class="menu menu-horizontal px-1">
        <li><NuxtLink to="/">Início</NuxtLink></li>
        <li><NuxtLink to="/cadastro">Cadastro</NuxtLink></li>
      </ul>
    </div>

    <NuxtPage />
  </div>
</template>