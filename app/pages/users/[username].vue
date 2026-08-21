<script setup lang="ts">

interface User {
  name: string;
  login: string;
  bio: string | null;
  avatar_url: string | undefined;
  html_url: string;
  followers?: number;
  following?: number;
  public_repos?: number;
  location?: string | null;
  blog?: string | null;
  company?: string | null;
}

interface Repo {
  id: number;
  name: string;
  full_name: string;
  description: string | null;
  html_url: string;
  stargazers_count: number;
  forks_count: number;
  language: string | null;
  updated_at: string;
  fork: boolean;
  archived: boolean;
}

const route = useRoute();
// it has to be a reactive value
const username = computed(() => route.params.username);

const { data: user, pending, error } = await useFetch<User>(
  () => `https://api.github.com/users/${username.value}`,
  {
    key: () => `github-user-${username.value}`,
    watch: [username],
  }
);

</script>

<template>
  <div v-if="pending" class="w-full h-screen">
    <p class="font-bold text-2xl">
      در حال دریافت اطلاعات
    </p>
  </div>
  <div v-else-if="error">
    <p v-if="error.statusCode === 404">کاربری با این نام پیدا نشد.</p>
    <p v-else-if="error.statusCode === 403">محدودیت درخواست به API گیت‌هاب. کمی صبر کن.</p>
    <p v-else>خطایی رخ داد.</p>
  </div>
  <div v-else-if="user">
    <img :src="user.avatar_url" class="w-20 h-20 rounded-full" />
    <h2>{{ user.name ?? user.login }}</h2>
    <p>{{ user.bio }}</p>
  </div>
</template>