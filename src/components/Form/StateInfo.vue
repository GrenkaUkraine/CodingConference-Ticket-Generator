<script setup lang="ts">
import IconInfo from '../../assets/icons/icon-info.svg';
import { onMounted, ref } from 'vue';

const { 
  error = false, 
  noInitialAnimation = false 
} = defineProps<{
    error?: boolean;
    noInitialAnimation?: boolean;
}>();

const isVisible = ref(noInitialAnimation);

const hide = () => {
  isVisible.value = false;
};

const show = () => {
  isVisible.value = true;
};

onMounted(() => {
  if (!noInitialAnimation) {
    setTimeout(() => {
      isVisible.value = true;
    }, 50);
  }
});

defineExpose({
  hide,
  show
});
</script>

<template>
    <div class="w-full flex items-center justify-start gap-2 state-info-animation overflow-hidden transition-all duration-100"
        :class="[
          error ? 'text-error' : 'text-text-secondary',
          isVisible ? 'state-info-visible' : 'state-info-hidden'
        ]">
        <IconInfo />
        <slot />
    </div>
</template>

<style scoped>
.state-info-animation {
  transition: all 0.3s ease-in-out;
}

.state-info-hidden {
  opacity: 0;
  transform: translateY(10px);
  max-height: 0;
  margin: 0;
}

.state-info-visible {
  opacity: 1;
  transform: translateY(0);
  max-height: 24px;
  margin: 0;
}
</style>
