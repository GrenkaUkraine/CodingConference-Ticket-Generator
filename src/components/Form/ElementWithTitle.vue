<script setup lang="ts">
defineProps<{
    title: string
}>();
</script>

<template>
    <div class="w-full flex flex-col items-start justify-start gap-3">
        <h3>{{ title }}</h3>
        <slot />
    </div>
</template>