<script setup lang="ts">
import IconUpload from '../../assets/icons/icon-upload.svg';
</script>

<template>
    <div class="w-full h-32 flex flex-col items-center justify-center gap-4 glass-element rounded-xl !border-none dashed-border">
        <div class="w-13 h-13 rounded-xl glass-element flex items-center justify-center">
            <IconUpload />
        </div>
        <h3 class="text-center !font-normal tracking-[-1px]">Drag and drop or click to upload</h3>
    </div>
</template>