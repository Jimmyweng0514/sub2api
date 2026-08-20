<template>
  <div v-if="hasHomeContent" class="min-h-screen">
    <iframe v-if="isHomeContentUrl" :src="homeContent.trim()" class="h-screen w-full border-0" allowfullscreen></iframe>
    <div v-else v-html="homeContent"></div>
  </div>

  <div v-else-if="compactHomeEnabled" data-testid="compact-home" class="flex min-h-screen flex-col bg-[#f4f7fb] text-gray-950 dark:bg-dark-950 dark:text-white">
    <header class="border-b border-gray-200 bg-white px-4 py-3 dark:border-dark-700 dark:bg-dark-950 sm:px-6">
      <nav class="mx-auto flex max-w-5xl flex-wrap items-center justify-between gap-3">
        <div class="flex min-w-0 items-center gap-3">
          <img :src="siteLogo || '/logo.svg'" alt="Logo" class="h-9 w-9 rounded-lg" />
          <span class="truncate text-base font-bold">{{ siteName }}</span>
        </div>
        <div class="flex items-center gap-2">
          <LocaleSwitcher />
          <button class="btn btn-ghost btn-icon" :title="isDark ? t('home.switchToLight') : t('home.switchToDark')" @click="toggleTheme">
            <Icon :name="isDark ? 'sun' : 'moon'" size="md" />
          </button>
          <router-link :to="isAuthenticated ? dashboardPath : '/login'" class="btn btn-primary">{{ isAuthenticated ? t('home.dashboard') : t('home.login') }}</router-link>
        </div>
      </nav>
    </header>
    <main class="flex flex-1 items-center justify-center px-4 py-16">
      <div class="max-w-2xl text-center">
        <img :src="siteLogo || '/logo.svg'" alt="Logo" class="mx-auto mb-6 h-20 w-20 rounded-xl" />
        <h1 class="[overflow-wrap:anywhere] text-3xl font-bold md:text-4xl">{{ siteName }}</h1>
        <p class="mt-4 whitespace-pre-wrap text-base text-gray-600 dark:text-dark-300">{{ siteSubtitle }}</p>
        <router-link :to="isAuthenticated ? dashboardPath : '/login'" class="btn btn-primary mt-8">{{ isAuthenticated ? t('home.goToDashboard') : t('home.login') }}</router-link>
      </div>
    </main>
    <footer class="border-t border-gray-200 px-4 py-5 text-center text-xs text-gray-500 dark:border-dark-700 dark:text-dark-400">&copy; {{ currentYear }} {{ siteName }}</footer>
  </div>

  <div v-else class="min-h-screen bg-[#f4f7fb] text-gray-950 dark:bg-dark-950 dark:text-white">
    <header class="sticky top-0 z-30 border-b border-gray-200 bg-white/95 px-4 py-3 backdrop-blur dark:border-dark-700 dark:bg-dark-950/95 sm:px-6">
      <nav class="mx-auto flex max-w-6xl items-center justify-between gap-4">
        <div class="flex min-w-0 items-center gap-3">
          <img :src="siteLogo || '/logo.svg'" alt="Logo" class="h-9 w-9 rounded-lg" />
          <div class="min-w-0">
            <div class="truncate text-sm font-bold">{{ siteName }}</div>
            <div class="hidden font-mono text-[10px] text-gray-400 sm:block">RELAY CONTROL PLANE</div>
          </div>
        </div>
        <div class="flex items-center gap-1 sm:gap-2">
          <div class="mr-2 hidden items-center gap-2 text-xs text-gray-500 md:flex dark:text-dark-400">
            <span class="h-2 w-2 rounded-full bg-emerald-500"></span> All systems operational
          </div>
          <LocaleSwitcher />
          <a v-if="docUrl" :href="docUrl" target="_blank" rel="noopener noreferrer" class="btn btn-ghost btn-icon" :title="t('home.viewDocs')"><Icon name="book" size="md" /></a>
          <button class="btn btn-ghost btn-icon" :title="isDark ? t('home.switchToLight') : t('home.switchToDark')" @click="toggleTheme"><Icon :name="isDark ? 'sun' : 'moon'" size="md" /></button>
          <router-link :to="isAuthenticated ? dashboardPath : '/login'" class="btn btn-primary"><span class="hidden sm:inline">{{ isAuthenticated ? t('home.dashboard') : t('home.login') }}</span><Icon name="arrowRight" size="sm" /></router-link>
        </div>
      </nav>
    </header>

    <main>
      <section class="relative overflow-hidden border-b border-gray-200 bg-white dark:border-dark-700 dark:bg-dark-950">
        <div class="absolute inset-0 bg-mesh-gradient bg-[size:32px_32px]"></div>
        <div class="relative mx-auto grid min-h-[620px] max-w-6xl content-center gap-12 px-4 py-16 sm:px-6 lg:grid-cols-[1.08fr_.92fr] lg:items-center">
          <div class="max-w-2xl">
            <div class="mb-5 inline-flex items-center gap-2 rounded border border-primary-200 bg-primary-50 px-2.5 py-1 font-mono text-xs font-semibold text-primary-700 dark:border-primary-800 dark:bg-primary-950 dark:text-primary-300">
              <span>BLUEFUTURE / EDGE 01</span>
            </div>
            <h1 class="text-4xl font-bold leading-[1.08] sm:text-5xl lg:text-6xl">更快的请求路径，<br /><span class="text-primary-600 dark:text-primary-400">更稳的 AI 分发。</span></h1>
            <p class="mt-6 max-w-xl text-base leading-7 text-gray-600 sm:text-lg dark:text-dark-300">{{ siteSubtitle }}。统一管理多模型、多渠道、密钥、额度和固定出口，把每一次转发变成可追踪、可控制的基础设施。</p>
            <div class="mt-8 flex flex-wrap gap-3">
              <router-link :to="isAuthenticated ? dashboardPath : '/login'" class="btn btn-primary btn-lg">{{ isAuthenticated ? t('home.goToDashboard') : t('home.getStarted') }}<Icon name="arrowRight" size="md" /></router-link>
              <router-link to="/model-plaza" class="btn btn-secondary btn-lg"><Icon name="grid" size="md" />模型广场</router-link>
            </div>
            <div class="mt-10 flex flex-wrap gap-x-6 gap-y-3 text-xs font-medium text-gray-500 dark:text-dark-400">
              <span v-for="item in assurances" :key="item" class="flex items-center gap-2"><Icon name="checkCircle" size="sm" class="text-emerald-500" />{{ item }}</span>
            </div>
          </div>

          <div class="terminal-container w-full">
            <div class="overflow-hidden rounded-lg border border-[#24364f] bg-[#07111f] shadow-2xl shadow-primary-950/20">
              <div class="flex items-center justify-between border-b border-[#24364f] px-4 py-3">
                <div class="flex items-center gap-2 font-mono text-xs text-slate-400"><span class="h-2 w-2 rounded-full bg-emerald-400"></span> live request</div>
                <span class="font-mono text-[10px] text-slate-500">req_7F2A91</span>
              </div>
              <div class="space-y-5 p-5 font-mono text-xs sm:p-6 sm:text-sm">
                <div class="grid grid-cols-[86px_1fr] gap-3"><span class="text-slate-500">endpoint</span><span class="text-sky-300">POST /v1/responses</span></div>
                <div class="grid grid-cols-[86px_1fr] gap-3"><span class="text-slate-500">model</span><span class="text-white">gpt-5 / auto-route</span></div>
                <div class="grid grid-cols-[86px_1fr] gap-3"><span class="text-slate-500">egress</span><span class="text-white">tokyo-edge-02</span></div>
                <div class="h-px bg-[#24364f]"></div>
                <div class="grid grid-cols-3 gap-3">
                  <div v-for="metric in liveMetrics" :key="metric.label" class="rounded border border-[#24364f] bg-[#0d1929] p-3">
                    <div class="text-base font-bold" :class="metric.color">{{ metric.value }}</div>
                    <div class="mt-1 text-[10px] uppercase text-slate-500">{{ metric.label }}</div>
                  </div>
                </div>
                <div class="flex items-center gap-3 rounded border border-emerald-900/70 bg-emerald-950/40 px-3 py-2 text-emerald-300"><Icon name="checkCircle" size="sm" />Stream established · HTTP 200</div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section class="border-b border-gray-200 bg-[#07111f] text-white dark:border-dark-700">
        <div class="mx-auto grid max-w-6xl grid-cols-2 gap-px bg-[#24364f] sm:grid-cols-4">
          <div v-for="stat in platformStats" :key="stat.label" class="bg-[#07111f] px-5 py-6">
            <div class="font-mono text-xl font-bold">{{ stat.value }}</div>
            <div class="mt-1 text-xs text-slate-400">{{ stat.label }}</div>
          </div>
        </div>
      </section>

      <section class="mx-auto max-w-6xl px-4 py-16 sm:px-6">
        <div class="mb-8 max-w-2xl">
          <p class="font-mono text-xs font-semibold text-primary-600 dark:text-primary-400">BUILT FOR OPERATORS</p>
          <h2 class="mt-3 text-2xl font-bold sm:text-3xl">中转站需要的是控制力，不是装饰。</h2>
          <p class="mt-3 text-sm leading-6 text-gray-600 dark:text-dark-300">从账号调度到使用记录，关键状态集中在同一个安静、清晰的工作界面。</p>
        </div>
        <div class="grid gap-px overflow-hidden rounded-lg border border-gray-200 bg-gray-200 md:grid-cols-3 dark:border-dark-700 dark:bg-dark-700">
          <article v-for="feature in features" :key="feature.title" class="bg-white p-6 dark:bg-dark-900">
            <div class="mb-5 flex h-10 w-10 items-center justify-center rounded-md" :class="feature.iconClass"><Icon :name="feature.icon" size="md" :stroke-width="2" /></div>
            <h3 class="font-semibold">{{ feature.title }}</h3>
            <p class="mt-2 text-sm leading-6 text-gray-600 dark:text-dark-300">{{ feature.description }}</p>
          </article>
        </div>
      </section>
    </main>

    <footer class="border-t border-gray-200 bg-white px-4 py-6 dark:border-dark-700 dark:bg-dark-950 sm:px-6">
      <div class="mx-auto flex max-w-6xl flex-col justify-between gap-3 text-xs text-gray-500 sm:flex-row dark:text-dark-400">
        <span>&copy; {{ currentYear }} {{ siteName }} · AI relay infrastructure</span>
        <div class="flex gap-4"><a v-if="docUrl" :href="docUrl" target="_blank" rel="noopener noreferrer">{{ t('home.docs') }}</a><a :href="githubUrl" target="_blank" rel="noopener noreferrer">GitHub</a></div>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import { useI18n } from 'vue-i18n'
import { useAuthStore, useAppStore } from '@/stores'
import LocaleSwitcher from '@/components/common/LocaleSwitcher.vue'
import Icon from '@/components/icons/Icon.vue'
import { sanitizeUrl } from '@/utils/url'

const { t } = useI18n()
const authStore = useAuthStore()
const appStore = useAppStore()
const siteName = computed(() => appStore.cachedPublicSettings?.site_name || appStore.siteName || 'BlueFuture')
const siteLogo = computed(() => sanitizeUrl(appStore.cachedPublicSettings?.site_logo || appStore.siteLogo || '', { allowRelative: true, allowDataUrl: true }))
const siteSubtitle = computed(() => appStore.cachedPublicSettings?.site_subtitle || 'Stable, observable AI relay infrastructure')
const docUrl = computed(() => sanitizeUrl(appStore.cachedPublicSettings?.doc_url || appStore.docUrl || ''))
const homeContent = computed(() => appStore.cachedPublicSettings?.home_content || '')
const hasHomeContent = computed(() => homeContent.value.trim().length > 0)
const compactHomeEnabled = computed(() => appStore.cachedPublicSettings?.compact_home_enabled === true)
const isHomeContentUrl = computed(() => /^https?:\/\//.test(homeContent.value.trim()))
const isDark = ref(document.documentElement.classList.contains('dark'))
const githubUrl = 'https://github.com/Jimmyweng0514/sub2api'
const isAuthenticated = computed(() => authStore.isAuthenticated)
const dashboardPath = computed(() => authStore.isAdmin ? '/admin/dashboard' : '/dashboard')
const currentYear = computed(() => new Date().getFullYear())
const assurances = ['SSE / WebSocket ready', '固定出口与故障隔离', '精细额度与审计日志']
const liveMetrics = [
  { value: '186ms', label: 'TTFT', color: 'text-sky-300' },
  { value: '32.4', label: 'TOK/S', color: 'text-white' },
  { value: '99.98%', label: 'HEALTH', color: 'text-emerald-300' }
]
const platformStats = [
  { value: '4+', label: '主流模型协议' },
  { value: 'SSE', label: '原生流式传输' },
  { value: 'HA', label: '有界重试与熔断' },
  { value: '24/7', label: '渠道健康监测' }
]
const features = [
  { icon: 'swap' as const, title: '统一协议入口', description: '兼容 OpenAI、Claude、Gemini 等客户端，让调用方只维护一个稳定端点。', iconClass: 'bg-primary-50 text-primary-700 dark:bg-primary-950 dark:text-primary-300' },
  { icon: 'server' as const, title: '健康感知调度', description: '根据额度、并发、错误率和固定代理约束选择上游，异常账号及时隔离。', iconClass: 'bg-emerald-50 text-emerald-700 dark:bg-emerald-950 dark:text-emerald-300' },
  { icon: 'chart' as const, title: '用量与性能可见', description: '请求、Token、费用、首 Token 时间和错误链路集中记录，方便运营和排障。', iconClass: 'bg-amber-50 text-amber-700 dark:bg-amber-950 dark:text-amber-300' }
]

function toggleTheme() {
  isDark.value = !isDark.value
  document.documentElement.classList.toggle('dark', isDark.value)
  localStorage.setItem('theme', isDark.value ? 'dark' : 'light')
}

onMounted(() => {
  const savedTheme = localStorage.getItem('theme')
  if (savedTheme === 'dark' || (!savedTheme && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
    isDark.value = true
    document.documentElement.classList.add('dark')
  }
  authStore.checkAuth()
  if (!appStore.publicSettingsLoaded) appStore.fetchPublicSettings()
})
</script>
