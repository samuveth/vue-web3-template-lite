<script setup lang="ts">
import { ref, computed } from 'vue'
import { useWeb3 } from '@/composables/useWeb3'
import { connectors } from '@/helpers/connectors'
import { useModalConnect } from '@/composables/useModalConnect'
import { refDebounced } from '@vueuse/core'
import { getInjectedWallet } from 'vite-plugin-vue-lock'

const { isConnectOpen, closeConnect } = useModalConnect()
const { login, web3Account } = useWeb3()

const loading = ref(false)
const loadingDebounced = refDebounced(loading)

async function handleSelect(connectorId: string) {
  loading.value = true
  await login(connectorId)
  loading.value = false
  if (web3Account.value) return closeConnect()
}

const injectedWallet = computed(() => getInjectedWallet())
</script>

<template>
  <BaseModal
    :is-open="isConnectOpen"
    title="Available wallets"
    @close="closeConnect"
  >
    <template #body>
      <div v-if="loadingDebounced" class="h-[140px] gap-2 text-center">
        <div class="flex h-full items-center justify-center">
          <i-s-spinner class="h-10 w-10 animate-spin" />
        </div>
      </div>
      <div v-else class="grid grid-cols-1 gap-3 sm:grid-cols-2">
        <div
          v-for="(connector, id, i) in connectors"
          :key="i"
          @click="handleSelect(connector.id)"
        >
          <div
            class="btn btn-outline flex gap-2 text-xs"
            v-if="id === 'injected' && injectedWallet"
          >
            <i-s-metamask class="text-xl" />
            {{ injectedWallet.name }}
          </div>
          <div
            class="btn btn-outline flex gap-2 text-xs"
            v-else-if="id !== 'injected'"
          >
            <i-s-walletconnect
              v-if="connector.id === 'walletconnect'"
              class="h-5"
            />
            <span>{{ connector.name }}</span>
          </div>
        </div>
      </div>
    </template>
  </BaseModal>
</template>
