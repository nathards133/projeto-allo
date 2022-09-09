<template>
    <aside :class="`${is_expanded ? 'is-expanded' : ''}`">
      <div class="logoA" v-if="is_expanded">
      <img :src="logoURL" alt="Vue" />
    </div>
      <div class="logoB" v-else>
      <img :src="logoURL" alt="Vue" />
    </div>

      <h3>Menu</h3>
      <div class="menu">
        <router-link to="/" class="button">
          <span class="fas fa-home"></span>
          <span class="text ml-2"> Home</span>
        </router-link>
        <router-link to="/register" class="button">
          <i class="fa-solid fa-user-plus"></i>
          <span class="text ml-2"> Registrar</span>
        </router-link>
        <router-link to="/settings" class="button">
          <i class="fa-solid fa-gear"></i>
          <span class="text ml-2"> Settings</span>
        </router-link>


      </div>
      <div class="flex"></div>

      <div class="menu-toggle-wrap">
        <button class="menu-toggle" @click="ToggleMenu">
          <i class="fa-solid fa-arrow-right"></i>
        </button>
      </div>
      <div class="container">
        <i class="fa-solid fa-user"></i>
      </div>
    </aside>
</template>

<script setup>
import { ref } from "vue";
import logoURL from "../assets/logo.png";

const is_expanded = ref(localStorage.getItem("is_expanded") === "true");

const ToggleMenu = () => {
  is_expanded.value = !is_expanded.value;
  localStorage.setItem("is_expanded", is_expanded.value);
};
</script>

<style lang="scss" scoped>
aside {
  display: flex;
  flex-direction: column;

  background-color: var(--dark);
  color: var(--light);

  width: calc(2rem + 28px);
  overflow: hidden;
  min-height: 100vh;
  padding: 1rem;

  transition: 0.2s ease-in-out;

  .flex {
    flex: 1 1 0%;
  }

  .logo {
    margin-bottom: 1rem;

    img {
      width: 3rem;
    }
  }
  .logoA {
    img {
      width: 3rem;
    }
  }
  .logoB {
    img {
      width: 2.5rem;
    }
  }


  .menu-toggle-wrap {
    display: flex;
    justify-content: flex-end;
    margin-bottom: 1rem;
    position: relative;
    top: 0;
    transition: 0.2s ease-in-out;
    .menu-toggle {
      transition: 0.2s ease-in-out;
      color: var(--primary)
    }
  }
  h3, .button .text {
    opacity: 0;
    transition: opacity 0.3s ease-in-out;
  }

  h3 {
    color: var(--grey);
    font-size: 0.875rem;
    margin-bottom: 0.5rem;
    text-transform: uppercase;
  }

  .menu {
    margin: 0 -1rem;

    .button {
      display: flex;
      align-items: center;
      text-decoration: none;

      transition: 0.2s ease-in-out;
      padding: 0.5rem 1rem;

      .text {
        color: var(--light);
        transition: 0.2s ease-in-out;
      }

      &:hover {
        background-color: var(--dark-alt);

        .material-icons,
        .text {
          color: var(--primary);
        }
      }

      &.router-link-exact-active {
        background-color: var(--dark-alt);
        border-right: 5px solid var(--primary);
      }
    }
  }

  .footer {
    opacity: 0;
    transition: opacity 0.3s ease-in-out;

    p {
      font-size: 0.875rem;
      color: var(--grey);
    }
  }

  &.is-expanded {
    width: var(--sidebar-width);

    .menu-toggle {
      transform: rotate(-180deg);
      color: var(--primary);
    }

    h3,
    .button .text {
      opacity: 1;
    }

    .footer {
      opacity: 0;
    }
  }

  @media (max-width: 1024px) {
    position: fixed;
    z-index: 99;
  }
}
</style>
