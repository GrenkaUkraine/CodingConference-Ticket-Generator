<script setup lang="ts">
import { computed, ref, watch } from 'vue';

interface Props {
    placeholder?: string;
    textTemplate?: string;
    modelValue?: string;
    textTemplateOnlyRender?: boolean;
    hideTemplateWhenEmpty?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
    placeholder: '',
    textTemplate: '',
    modelValue: '',
    textTemplateOnlyRender: false,
    hideTemplateWhenEmpty: false
});

const emit = defineEmits<{
    'update:modelValue': [value: string];
    'enter': [];
}>();

const inputValue = ref('');

const formatText = (text: string, template?: string): string => {
    if (!template) return text;
    
    const placeholder = '%';
    if (template.includes(placeholder)) {
        return template.replace(placeholder, text);
    }
    
    return text;
};

const displayValue = computed(() => {
    if (inputValue.value === '' && props.hideTemplateWhenEmpty) {
        return '';
    }
    
    if (props.textTemplate) {
        const placeholder = '%';
        if (props.textTemplate.includes(placeholder)) {
            const prefix = props.textTemplate.split(placeholder)[0];
            return prefix + inputValue.value;
        }
    }
    
    return inputValue.value;
});

watch(() => props.modelValue, (newValue) => {
    if (newValue !== undefined) {
        if (props.textTemplate && props.textTemplateOnlyRender) {
            const placeholder = '%';
            if (props.textTemplate.includes(placeholder)) {
                const prefix = props.textTemplate.split(placeholder)[0];
                if (newValue.startsWith(prefix)) {
                    inputValue.value = newValue.substring(prefix.length);
                    return;
                }
            }
        }
        
        inputValue.value = newValue;
    }
}, { immediate: true });

const handleInput = (event: Event) => {
    const target = event.target as HTMLInputElement;
    inputValue.value = target.value;
    
    if (props.textTemplate && !props.textTemplateOnlyRender) {
        const formattedValue = formatText(inputValue.value, props.textTemplate);
        emit('update:modelValue', formattedValue);
    } else {
        emit('update:modelValue', inputValue.value);
    }
};

const handleKeydown = (event: KeyboardEvent) => {
    if (event.key === 'Enter') {
        emit('enter');
    }
};
</script>

<template>
    <input 
        class="w-full h-13.5 flex flex-col items-center justify-start px-4 py-2 glass-element text-text-primary placeholder:text-text-secondary rounded-xl" 
        :placeholder="placeholder"
        :value="displayValue"
        @input="handleInput"
        @keydown="handleKeydown"
    />
</template>