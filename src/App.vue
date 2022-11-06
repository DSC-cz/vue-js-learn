<template>
  <nav>
    <router-link to="/">Hlavní stránka</router-link>
    <router-link to="/todo">To do list</router-link>
  </nav>
  <main class="container">
    <router-view/>
  </main>
</template>
