<script setup>
import Github_icon from '~/assets/svgs/github_icon.vue';

const username = ref("");
const router = useRouter();

function handleSearch() {
  let trimmed = username.value.trim();
  if(!trimmed) return;

  router.push(`/users/${trimmed}`);
}
</script>

<template>
  <header class="bg-surface p-4">
    <div class="max-w-7xl mx-auto">
      <div class="flex justify-between px-3 py-1">
        <NuxtLink to="/">
          <div class="flex items-center justify-center">
            <Github_icon class="w-16 h-16"/>
            <span class="font-medium text-xl">مرورگر گیت‌هاب</span>
          </div>
        </NuxtLink>
        <div class="flex justify-center items-center">
          <form @submit.prevent="handleSearch" class="flex justify-center items-center">
            <div class="flex items-center gap-2 px-6 py-1 bg-background rounded-xl">
              <button type="submit" class="bg-important-button text-white text-sm px-3 py-1.5 rounded-lg">جستوجو</button>
              <input
                type="text"
                v-model="username"
                placeholder="جستوجو با نام‌کاربری"
                class="text-sm rounded-lg px-5 py-3 w-full focus:outline-none"
              />
            </div>
          </form>
        </div>
      </div>
    </div>  
  </header>
</template>