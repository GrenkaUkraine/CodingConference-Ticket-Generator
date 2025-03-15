<script setup lang="ts">
import IconInfo from '../../assets/icons/icon-info.svg';

const { error = false } = defineProps<{
    error?: boolean;
}>();
</script>

<template>
    <div class="w-full h-full flex items-center justify-start gap-2"
        :class="error ? 'text-error' : 'text-text-secondary'">
        <IconInfo />
        <slot />
    </div>
</template>
