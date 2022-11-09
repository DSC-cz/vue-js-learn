<template>
  <nav>
    <router-link to="/">Hlavní stránka</router-link>
    <router-link to="/todo">To do list</router-link>
    <router-link to="/calc">Kalkulačka</router-link>
  </nav>
  <main class="container">
    <router-view/>
  </main>
</template>
