<template>
  <header>
    <Container>
      <nav class="flex justify-between py-5">
        <nuxt-link to="/" class="font-bold">logo</nuxt-link>

        <ul class="flex gap-10">
          <li><nuxt-link to="/login">log in</nuxt-link></li>
          <li><nuxt-link to="/signup">sign up</nuxt-link></li>
        </ul>
      </nav>
    </Container>
  </header>
</template>

<script setup></script>

<style lang="scss" scoped></style>
