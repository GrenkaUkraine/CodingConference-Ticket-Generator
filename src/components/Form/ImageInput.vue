<script setup lang="ts">
import { ref, watch } from 'vue';
import IconUpload from '../../assets/icons/icon-upload.svg';
import ImageInputButton from './ImageInputButton.vue';
import StateInfo from './StateInfo.vue';

interface Props {
    modelValue?: string | null;
    error?: boolean;
    errorMessage?: string;
}

const props = withDefaults(defineProps<Props>(), {
    modelValue: null,
    error: false,
    errorMessage: ''
});

const emit = defineEmits<{
    'update:modelValue': [value: string | null];
}>();

const selectedImage = ref<string | null>(null);
const fileInput = ref<HTMLInputElement | null>(null);
const hasError = ref(false);
const errorMessage = ref('');

watch(() => props.modelValue, (newValue) => {
    selectedImage.value = newValue;
}, { immediate: true });

watch(() => selectedImage.value, (newValue) => {
    emit('update:modelValue', newValue);
});

const validateImage = (file: File): Promise<boolean> => {
    return new Promise((resolve) => {
        if (file.size > 500 * 1024) {
            errorMessage.value = 'File too large. Please upload a photo under 500KB.';
            hasError.value = true;
            resolve(false);
            return;
        }

        const img = new Image();
        const objectUrl = URL.createObjectURL(file);
        
        img.onload = () => {
            URL.revokeObjectURL(objectUrl);
            
            const aspectRatio = img.width / img.height;
            const isSquare = Math.abs(aspectRatio - 1) < 0.05;
            
            if (!isSquare) {
                errorMessage.value = 'Image not square. Please upload a photo with 1:1 ratio.';
                hasError.value = true;
                resolve(false);
                return;
            }
            
            hasError.value = false;
            resolve(true);
        };
        
        img.onerror = () => {
            URL.revokeObjectURL(objectUrl);
            errorMessage.value = 'Invalid image file. Please try another photo.';
            hasError.value = true;
            resolve(false);
        };
        
        img.src = objectUrl;
    });
};

const handleFileSelect = async (event: Event) => {
    const input = event.target as HTMLInputElement;
    if (input.files && input.files.length > 0) {
        const file = input.files[0];
        
        const isValid = await validateImage(file);
        
        if (isValid) {
            const reader = new FileReader();
            reader.onload = (e) => {
                selectedImage.value = e.target?.result as string;
                emit('update:modelValue', selectedImage.value);
            };
            reader.readAsDataURL(file);
        } else {
            emit('update:modelValue', null);
        }
    }
};

const openFileDialog = () => {
    if (fileInput.value) {
        fileInput.value.click();
    }
};

const removeImage = () => {
    selectedImage.value = null;
    hasError.value = false;
    if (fileInput.value) {
        fileInput.value.value = '';
    }
    emit('update:modelValue', null);
};

const changeImage = () => {
    openFileDialog();
};

const handleDragOver = (event: DragEvent) => {
    event.preventDefault();
};

const handleDrop = async (event: DragEvent) => {
    event.preventDefault();

    if (event.dataTransfer?.files && event.dataTransfer.files.length > 0) {
        const file = event.dataTransfer.files[0];
        
        const isValid = await validateImage(file);
        
        if (isValid) {
            const reader = new FileReader();
            reader.onload = (e) => {
                selectedImage.value = e.target?.result as string;
                emit('update:modelValue', selectedImage.value);
            };
            reader.readAsDataURL(file);
        } else {
            emit('update:modelValue', null);
        }
    }
};

const handleKeydown = (event: KeyboardEvent) => {
    if (event.key === 'Enter' && !selectedImage.value) {
        openFileDialog();
    }
};
</script>

<template>
    <div class="flex flex-col gap-3 w-full">
        <div class="w-full h-32 flex flex-col items-center justify-center gap-4 glass-element rounded-xl !border-none dashed-border select-none focus-outline"
            :class="{ 
                '!border-error': props.error, 
                'cursor-pointer glass-element-can-hover': !selectedImage 
            }"
            tabindex="0"
            @dragover="handleDragOver" 
            @drop="handleDrop" 
            @keydown="handleKeydown"
            @click="!selectedImage && openFileDialog()">
            <input type="file" ref="fileInput" @change="handleFileSelect" accept="image/*"
                class="hidden user-select-none" />

            <template v-if="!selectedImage">
                <button class="w-13 h-13 rounded-xl glass-element flex items-center justify-center select-none focus-outline"
                    tabindex="-1"
                    @click.stop="openFileDialog">
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
                    <ImageInputButton variant="primary" @click.stop="removeImage">Remove Image</ImageInputButton>
                    <ImageInputButton variant="secondary" @click.stop="changeImage">Change Image</ImageInputButton>
                </div>
            </template>
        </div>
        <StateInfo noInitialAnimation :error="hasError || props.error">
            {{ hasError ? errorMessage : props.error ? props.errorMessage : 'Upload a your photo (JPG or PNG, max 500KB).' }}
        </StateInfo>
    </div>
</template>