<script setup lang="ts">
import ElementWithTitle from './Form/ElementWithTitle.vue';
import ImageInput from './Form/ImageInput.vue';
</script>

<template>
    <div class="w-[60%] max-w-[480px] h-full flex flex-col items-center justify-start">
        <ElementWithTitle title="Personal Information">
            <ImageInput />
        </ElementWithTitle>
    </div>
</template>