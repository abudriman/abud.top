<template>
  <b-navbar
    :spaced="true"
    :shadow="true"
    :fixed-top="true"
    wrapper-class="container"
  >
    <template #brand>
      <b-navbar-item tag="router-link" :to="{ path: '/' }">
        <Abudriman />
      </b-navbar-item>
    </template>

    <template #end>
      <b-navbar-item tag="div">
        <div class="buttons">
          <a
            class="button is-primary"
            @click="$nuxt.$emit('open-modal', false)"
          >
            <strong>Sign up</strong>
          </a>
          <a class="button is-light" @click="$nuxt.$emit('open-modal', true)">
            Sign in
          </a>
        </div>
      </b-navbar-item>
    </template>
  </b-navbar>
</template>

<script>
import Abudriman from '@/assets/svg/abudriman-light.svg?inline'
export default {
  name: 'Navbar',
  components: { Abudriman },
}
</script>

<style lang="scss"></style>
