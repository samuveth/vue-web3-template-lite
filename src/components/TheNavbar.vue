<script setup lang="ts">
import DropdownAccount from './DropdownAccount.vue'
import { useWeb3 } from '@/composables/useWeb3'
import { useModalConnect } from '@/composables/useModalConnect'

const { web3Account } = useWeb3()
const { openConnect } = useModalConnect()
</script>

<template>
  <div class="navbar bg-base-100 shadow-xl">
    <div class="flex-1">
      <a class="btn btn-ghost text-xl normal-case">vue-web3-template-lite</a>
    </div>
    <div class="flex-none">
      <DropdownAccount v-if="web3Account">
        <label tabindex="0" class="avatar btn btn-ghost btn-circle">
          <div class="w-10 rounded-full">
            <img :src="`https://cdn.stamp.fyi/avatar/${web3Account}`" />
          </div>
        </label>
      </DropdownAccount>

      <button v-else @click="openConnect" class="btn btn-outline btn-active">
        Connect
      </button>
    </div>
  </div>
</template>
