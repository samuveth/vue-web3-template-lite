<script setup lang="ts">
import { Menu, MenuButton, MenuItems, MenuItem } from '@headlessui/vue'

defineProps<{
  dropdownItems: { name: string; action: string }[]
}>()

defineEmits(['action'])
</script>

<template>
  <Menu as="div" class="relative inline-block w-full text-left">
    <div>
      <MenuButton class="w-full">
        <slot />
      </MenuButton>
    </div>

    <transition
      enter-active-class="transition ease-out duration-100"
      enter-from-class="transform opacity-0 scale-95"
      enter-to-class="transform opacity-100 scale-100"
      leave-active-class="transition ease-in duration-75"
      leave-from-class="transform opacity-100 scale-100"
      leave-to-class="transform opacity-0 scale-95"
    >
      <MenuItems
        class="rounded-box absolute right-0 mt-2 origin-top-left border border-base-200 bg-base-100 p-2 focus:outline-none"
      >
        <div>
          <MenuItem
            v-for="item in dropdownItems"
            v-slot="{ active }"
            :key="item.name"
          >
            <a
              href="#"
              :class="[
                { 'bg-base-200': active },
                'block whitespace-nowrap rounded-lg px-4 py-2 text-sm text-base-content',
              ]"
              @click="$emit('action', item.action)"
            >
              {{ item.name }}
            </a>
          </MenuItem>
        </div>
      </MenuItems>
    </transition>
  </Menu>
</template>
