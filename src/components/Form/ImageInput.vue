<script setup lang="ts">
import { ref } from 'vue';
import IconUpload from '../../assets/icons/icon-upload.svg';
import ImageInputButton from './ImageInputButton.vue';

const selectedImage = ref<string | null>(null);
const fileInput = ref<HTMLInputElement | null>(null);

const handleFileSelect = (event: Event) => {
    const input = event.target as HTMLInputElement;
    if (input.files && input.files.length > 0) {
        const file = input.files[0];
        const reader = new FileReader();

        reader.onload = (e) => {
            selectedImage.value = e.target?.result as string;
        };

        reader.readAsDataURL(file);
    }
};

const openFileDialog = () => {
    if (fileInput.value) {
        fileInput.value.click();
    }
};

const removeImage = () => {
    selectedImage.value = null;
    if (fileInput.value) {
        fileInput.value.value = '';
    }
};

const changeImage = () => {
    openFileDialog();
};

const handleDragOver = (event: DragEvent) => {
    event.preventDefault();
};

const handleDrop = (event: DragEvent) => {
    event.preventDefault();

    if (event.dataTransfer?.files && event.dataTransfer.files.length > 0) {
        const file = event.dataTransfer.files[0];
        const reader = new FileReader();

        reader.onload = (e) => {
            selectedImage.value = e.target?.result as string;
        };

        reader.readAsDataURL(file);
    }
};
</script>

<template>
    <div class="w-full h-32 flex flex-col items-center justify-center gap-4 glass-element rounded-xl !border-none dashed-border user-select-none"
        @dragover="handleDragOver" @drop="handleDrop">
        <input type="file" ref="fileInput" @change="handleFileSelect" accept="image/*"
            class="hidden user-select-none" />

        <template v-if="!selectedImage">
            <button class="w-13 h-13 rounded-xl glass-element flex items-center justify-center select-none"
                @click="openFileDialog">
                <IconUpload />
            </button>
            <h3 class="text-center !font-normal tracking-[-1px] select-none">Drag and drop or click to upload</h3>
        </template>

        <template v-else>
            <div
                class="w-13 h-13 rounded-xl glass-element flex items-center justify-center overflow-hidden select-none">
                <img :src="selectedImage" alt="Selected image" class="w-full h-full object-cover">
            </div>
            <div class="flex items-center justify-center gap-2">
                <ImageInputButton variant="primary" @click="removeImage">Remove Image</ImageInputButton>
                <ImageInputButton variant="secondary" @click="changeImage">Change Image</ImageInputButton>
            </div>
        </template>
    </div>
</template>