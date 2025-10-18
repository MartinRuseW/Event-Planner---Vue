<template>
  <nav class="navbar navbar-expand-lg custom-navbar">
    <div class="container-fluid">
      <RouterLink :to="{ name: 'home' }">
        <img src="@/utils/logo/Logo.png" />
      </RouterLink>
      <div class="collapse navbar-collapse justify-content-end" id="navbarNav">
        <ul class="navbar-nav ml-auto text-center">

          <li v-if="isAdmin" class="nav-item px-2 pt-2 text-danger fw-bold fs-5">
            <i class="bi bi-shield-lock"></i> Admin
          </li>

          <li v-if="!isAdmin && isLoggedIn" class="nav-item px-2">
            <span class="nav-link fw-bold fs-5 text-danger">
              Hi, {{ uStore.currentUser.username }}!
            </span>
          </li>
          <li
            v-for="link in visibleLinks"
            :key="link.name"
            class="nav-item px-2"
          >
            <RouterLink
              :to="{ name: link.name }"
              class="nav-link fw-bold fs-5"
              :class="{ active: route.name === link.name }"
            >
              {{ link.label }}
            </RouterLink>
          </li>

          <li v-if="isLoggedIn" class="nav-item px-2">
            <button @click="handleLogout" class="btn custom-btn fw-bold fs-5">
              Logout
            </button>
          </li>

        </ul>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { useRouter, useRoute } from 'vue-router';
import { computed } from 'vue';
import { userStore } from '@/store/userStore.js';

const uStore = userStore();
const router = useRouter();
const route = useRoute();

const isLoggedIn = computed(() => uStore.isLoggedIn);
const isAdmin = computed(() => uStore.isAdmin);


const handleLogout = async () => {
  await uStore.logout();

  router.push({ name: 'login' });
};

const navLinks = computed(() => [
  { name: 'events', label: 'Events', show: true },
  { name: 'profile', label: 'Profile', show: isLoggedIn.value },
  { name: 'login', label: 'Login', show: !isLoggedIn.value },
]);

const visibleLinks = computed(() => navLinks.value.filter((l) => l.show));
</script>

<style scoped lang="scss">
.custom-navbar {
  background-color: $dark-gray;

  img {
    width: 100px;
    height: 75px;
  }

  .navbar-nav {
    .nav-item {
      padding: 0 0.5rem;

      .nav-link {
        color: $off-white;
        transition: color 0.15s ease-in-out, border-color 0.15s ease-in-out;

        &:hover {
          color: $vibrant-teal;
        }

        &.active {
          color: $vibrant-teal !important;
          border-bottom: 3px solid $vibrant-teal;
        }
      }

      .text-danger {
        color: $candy-apple-red !important;
      }

      .custom-btn {
        background-color: $dark-gray;
        color: $off-white;
        border: none;
        border-radius: 5px;
        padding: 8px 12px;
        cursor: pointer;
        transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out;

        &:hover {
          color: $vibrant-teal;
        }
      }
    }
  }
}
</style>
