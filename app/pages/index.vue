<script setup>
import GithubIcon from '~/assets/svgs/github_icon.vue';

const username = ref("");
const router = useRouter();

function handleSearch() {
  let trimmed = username.value.trim();
  if(!trimmed) return;

  router.push(`/users/${trimmed}`);
}
</script>

<template>
  <section class="flex items-center py-6">
    <div class="flex flex-col items-center justify-center py-12 w-full gap-6">
      <GithubIcon  class="w-70 h-70"/>
      <h1 class="text-5xl font-black jetbrains">Github Explorer</h1>
      <p class="text-muted max-w-200 text-center">کاربران و ریپوزیتوری‌های GitHub را جستجو و کاوش کنید، اطلاعات پروفایل‌ها، پروژه‌ها و جزئیات هر ریپوزیتوری را مشاهده کنید و به‌سادگی پروژه‌های موردنظر خود را پیدا کنید</p>
      <div class="border border-border rounded-lg p-2">
        <form @submit.prevent="handleSearch" class="flex">
          <button type="submit" class="px-4 py-2 bg-important-button rounded-md">جستوجو</button>
          <input v-model="username" placeholder="جستوجوی با نا‌م‌کاربری" type="text" class="py-۲ px-6 w-full focus:outline-none">
        </form>
      </div>
    </div>
  </section>
</template>