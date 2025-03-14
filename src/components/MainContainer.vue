<script setup lang="ts">
import PatternSquigglyLineBottom from '../assets/images/pattern-squiggly-line-bottom-desktop.svg';
import PatternSquigglyLineTop from '../assets/images/pattern-squiggly-line-top.svg';
import PatternLines from '../assets/images/pattern-lines.svg';
import PatternCircle from '../assets/images/pattern-circle.svg';
</script>

<template>
  <div class="w-screen min-h-screen relative">
    <div class="z-10">
      <slot></slot>
    </div>
    <div class="absolute top-0 left-0 w-full h-full user-select-none -z-10">
      <div class="w-full h-full object-cover z-0 bg-cover" style="background-image: url('/images/background-desktop.png')"></div>
      <PatternSquigglyLineBottom class="absolute bottom-0 left-0 z-2" />
      <PatternSquigglyLineTop class="absolute top-[10vh] right-0 z-2" />
      <PatternLines class="absolute left-0 top-0 w-full h-full z-1 mix-blend-overlay pointer-events-none" />
      <PatternCircle class="absolute right-[20vw] bottom-[15vh] z-1" />
    </div>
  </div>
</template>
