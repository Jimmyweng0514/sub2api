<template>
  <div class="min-h-screen bg-[#f4f7fb] text-gray-950 dark:bg-dark-950 dark:text-white">
    <header class="border-b border-gray-200 bg-white/95 px-4 py-3 backdrop-blur dark:border-dark-700 dark:bg-dark-950/95 sm:px-6">
      <div class="mx-auto flex max-w-6xl items-center justify-between gap-4">
        <router-link to="/" class="flex min-w-0 items-center gap-3">
          <img :src="siteLogo || '/logo.svg'" alt="BlueFuture" class="h-9 w-9 rounded-lg" />
          <div class="min-w-0">
            <div class="truncate text-sm font-bold">{{ siteName }}</div>
            <div class="hidden text-xs text-gray-500 dark:text-dark-400 sm:block">AI RELAY CONTROL PLANE</div>
          </div>
        </router-link>
        <div class="flex items-center gap-2 text-xs text-gray-500 dark:text-dark-400">
          <span class="h-2 w-2 rounded-full bg-emerald-500"></span>
          <span>Gateway online</span>
        </div>
      </div>
    </header>

    <main class="mx-auto grid min-h-[calc(100vh-65px)] max-w-6xl items-center gap-10 px-4 py-10 sm:px-6 lg:grid-cols-[1fr_420px] lg:py-16">
      <section class="hidden max-w-xl lg:block">
        <p class="mb-4 font-mono text-xs font-semibold uppercase text-primary-600 dark:text-primary-400">BlueFuture / Secure access</p>
        <h1 class="text-4xl font-bold leading-tight">One access point.<br />Every AI route under control.</h1>
        <p class="mt-5 max-w-lg text-base leading-7 text-gray-600 dark:text-dark-300">统一管理密钥、额度、渠道健康度与请求追踪。登录后进入低延迟、可观测的分发控制面。</p>
        <div class="mt-8 grid grid-cols-3 gap-px overflow-hidden rounded-lg border border-gray-200 bg-gray-200 dark:border-dark-700 dark:bg-dark-700">
          <div v-for="metric in metrics" :key="metric.label" class="bg-white p-4 dark:bg-dark-900">
            <div class="font-mono text-sm font-bold text-gray-950 dark:text-white">{{ metric.value }}</div>
            <div class="mt-1 text-xs text-gray-500 dark:text-dark-400">{{ metric.label }}</div>
          </div>
        </div>
      </section>

      <section class="mx-auto w-full max-w-[420px]">
        <div class="mb-6 lg:hidden">
          <h1 class="text-2xl font-bold">{{ siteName }}</h1>
          <p class="mt-1 text-sm text-gray-500 dark:text-dark-400">{{ siteSubtitle }}</p>
        </div>
        <div class="rounded-lg border border-gray-200 bg-white p-6 shadow-card sm:p-8 dark:border-dark-700 dark:bg-dark-900">
          <slot />
        </div>
        <div class="mt-5 text-center text-sm"><slot name="footer" /></div>
        <div class="mt-6 text-center text-xs text-gray-400 dark:text-dark-500">&copy; {{ currentYear }} {{ siteName }}</div>
      </section>
    </main>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted } from 'vue'
import { useAppStore } from '@/stores'
import { sanitizeUrl } from '@/utils/url'

const appStore = useAppStore()
const siteName = computed(() => appStore.siteName || 'BlueFuture')
const siteLogo = computed(() => sanitizeUrl(appStore.siteLogo || '', { allowRelative: true, allowDataUrl: true }))
const siteSubtitle = computed(() => appStore.cachedPublicSettings?.site_subtitle || 'Stable, observable AI relay infrastructure')
const currentYear = computed(() => new Date().getFullYear())
const metrics = [
  { value: 'SSE', label: '流式优先' },
  { value: 'HA', label: '故障隔离' },
  { value: 'TLS', label: '安全传输' }
]

onMounted(() => appStore.fetchPublicSettings())
</script>
