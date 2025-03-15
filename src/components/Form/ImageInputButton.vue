<script setup lang="ts">
interface Props {
    disabled?: boolean;
    type?: 'button' | 'submit' | 'reset';
    variant?: 'primary' | 'secondary';
}

const props = withDefaults(defineProps<Props>(), {
    disabled: false,
    type: 'button',
    variant: 'secondary'
});

const emit = defineEmits<{
    'click': [event: MouseEvent];
}>();

const handleClick = (event: MouseEvent) => {
    if (!props.disabled) {
        emit('click', event);
    }
};
</script>

<template>
    <button :type="type" :disabled="disabled" @click="handleClick"
        class="w-full h-5.5 flex items-center justify-center rounded-sm px-2 text-xs font-medium glass-element text-text-primary tracking-[-0.24px] focus-outline"
        :class="[
            variant === 'primary' ? 'underline decoration-text-primary underline-offset-2' : ''
        ]">
        <slot />
    </button>
</template>