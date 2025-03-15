<script setup lang="ts">
interface Props {
    disabled?: boolean;
    type?: 'button' | 'submit' | 'reset';
}

const props = withDefaults(defineProps<Props>(), {
    disabled: false,
    type: 'button'
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
        class="w-full h-13.5 flex items-center justify-center rounded-xl font-bold text-xl bg-accent text-black focus-outline hover:bg-accent-hover border-accent hover:border-b-4 transition-all active:scale-99 active:border-none active:!outline-none">
        <slot />
    </button>
</template>
