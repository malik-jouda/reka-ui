<script lang="ts">
import type { Ref } from 'vue'
import type { PrimitiveProps } from '@/Primitive'
import { createContext } from '@/shared'
import type { CheckedState } from './utils'

interface MenuItemIndicatorContext {
  modelValue: Ref<CheckedState>
}

export interface MenuItemIndicatorProps extends PrimitiveProps {
  /**
   * Used to force mounting when more control is needed. Useful when
   * controlling animation with Vue animation libraries.
   */
  forceMount?: boolean
}

export const [injectMenuItemIndicatorContext, provideMenuItemIndicatorContext]
  = createContext<MenuItemIndicatorContext>(
    ['MenuCheckboxItem', 'MenuRadioItem'],
    'MenuItemIndicatorContext',
  )
</script>

<script setup lang="ts">
import { ref } from 'vue'
import { getCheckedState, isIndeterminate } from './utils'
import { Primitive } from '@/Primitive'
import { Presence } from '@/Presence'

withDefaults(defineProps<MenuItemIndicatorProps>(), {
  as: 'span',
})

const indicatorContext = injectMenuItemIndicatorContext({
  modelValue: ref(false),
})
</script>

<template>
  <Presence
    :present="
      forceMount
        || isIndeterminate(indicatorContext.modelValue.value)
        || indicatorContext.modelValue.value === true
    "
  >
    <Primitive
      :as="as"
      :as-child="asChild"
      :data-state="getCheckedState(indicatorContext.modelValue.value)"
    >
      <slot />
    </Primitive>
  </Presence>
</template>
