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

const { data: repos, pending: reposPending, error: reposError } = useFetch<Repo[]>(
  () => `https://api.github.com/users/${username.value}/repos`,
  {
    key: () => `repo-${username.value}`,
    watch: [username],
    query: {
      sort: "updated",
      per_page: 100,
    },
  }
)

const sortedRepos = computed(() => {
  if(!repos.value) return [];
  return [...repos.value].sort((a,b) => {
    return b.stargazers_count - a.stargazers_count
  });
})

// AI generated
const languageColors: Record<string, string> = {
  JavaScript: "#f1e05a",
  TypeScript: "#3178c6",
  Python: "#3572A5",
  Vue: "#41b883",
  HTML: "#e34c26",
  CSS: "#563d7c",
  Java: "#b07219",
  Go: "#00ADD8",
  Rust: "#dea584",
  C: "#555555",
  "C++": "#f34b7d",
  "C#": "#178600",
  PHP: "#4F5D95",
  Shell: "#89e051",
  Ruby: "#701516",
  Swift: "#F05138",
  Kotlin: "#A97BFF",
  Dart: "#00B4AB",
};

function languageColor(lang: string | null) {
  if (!lang) return "#8b949e";
  return languageColors[lang] ?? "#8b949e";
}

function formatDate(dateStr: string) {
  return new Intl.DateTimeFormat("fa-IR", {
    year: "numeric",
    month: "long",
    day: "numeric",
  }).format(new Date(dateStr));
}

function formatNumber(n: number | undefined) {
  if (n === undefined) return "0";
  return new Intl.NumberFormat("en-US").format(n);
}
// Until here


</script>

<template>
  <div class="min-h-screen px-4 py-10">
    <!-- sdcsdcmlsdl -->
    <div v-if="pending" class="flex flex-col items-center justify-center py-24 gap-4">
      <div class="w-12 h-12 border-4 border-gray-300 border-t-gray-800 rounded-full animate-spin" />
      <p class="font-bold text-xl">در حال دریافت اطلاعات</p>
    </div>

    <div v-else-if="error" class="flex flex-col items-center justify-center py-24 gap-3">
      <div class="text-5xl">❌</div>
      <p v-if="error.statusCode === 404" class="text-lg font-semibold">
        کاربری با این نام پیدا نشد.
      </p>
      <p v-else-if="error.statusCode === 403" class="text-lg font-semibold">
        محدودیت درخواست به API گیت‌هاب. کمی صبر کن.
      </p>
      <p v-else class="text-lg font-semibold">خطایی رخ داد.</p>
    </div>

    <div v-else-if="user">
      <div class="bg-surface rounded-2xl shadow-sm border border-border p-6 sm:p-8 flex flex-col sm:flex-row gap-6 items-center sm:items-start text-center sm:text-right">
        <img
          :src="user.avatar_url"
          :alt="user.login"
          class="w-28 h-28 sm:w-32 sm:h-32 rounded-full ring-4 ring-border shrink-0 object-cover"
        />

        <div class="flex-1 flex flex-col gap-2 w-full">
          <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2">
            <div>
              <h2 class="text-2xl font-bold text-foreground">{{ user.name ?? user.login }}</h2>
              <p class="text-muted">@{{ user.login }}</p>
            </div>

            <a
              :href="user.html_url"
              target="_blank"
              rel="noopener noreferrer"
              class="inline-flex items-center justify-center gap-2 px-4 py-2 rounded-lg bg-important-button text-sm font-medium hover:opacity-90 transition"
            >
              مشاهده در گیت‌هاب
            </a>
          </div>

          <p v-if="user.bio" class="text-foreground leading-relaxed">
            {{ user.bio }}
          </p>

          <div class="flex flex-wrap gap-4 mt-2 text-sm text-muted">
            <span v-if="user.company">🏢 {{ user.company }}</span>
            <span v-if="user.location">📍 {{ user.location }}</span>
            <a
              v-if="user.blog"
              :href="user.blog"
              target="_blank"
              rel="noopener noreferrer"
              class="hover:underline"
            >
              🔗 {{ user.blog }}
            </a>
          </div>

          
          <div class="flex gap-6 mt-2 pt-3 border-t border-border text-sm">
            <div>
              <span class="font-bold text-foreground">{{ formatNumber(user.public_repos) }}</span>
              <span class="text-muted"> مخزن</span>
            </div>
            <div>
              <span class="font-bold text-foreground">{{ formatNumber(user.followers) }}</span>
              <span class="text-muted"> دنبال‌کننده</span>
            </div>
            <div>
              <span class="font-bold text-foreground">{{ formatNumber(user.following) }}</span>
              <span class="text-muted"> دنبال‌شونده</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>