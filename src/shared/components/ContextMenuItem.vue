<template>
  <BDropdownItemButton
    :disabled="disabled"
    :button-class="[
      'd-flex justify-content-between align-items-center text-nowrap',
      {[`dropdown-item-${variant}`]: variant}
    ]"
    v-on="$listeners"
  >
    <slot>
      {{ text }}
    </slot>
    <Icon v-if="icon" :icon="icon" class="ms-4" />
  </BDropdownItemButton>
</template>
<script lang="ts">
  import { defineComponent } from 'vue'

  export default defineComponent({
    props: {
      text: { type: String, default: '' },
      icon: { type: String, default: null },
      variant: { type: String, default: '' },
      disabled: { type: Boolean, default: false },
    }
  })
</script>
<style>
  .dropdown-item-danger:not(:disabled) {
    color: var(--bs-danger);
  }
</style>
