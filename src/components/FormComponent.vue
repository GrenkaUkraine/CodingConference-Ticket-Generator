<script setup lang="ts">
import { ref } from 'vue';
import ElementWithTitle from './Form/ElementWithTitle.vue';
import ImageInput from './Form/ImageInput.vue';
import TextInput from './Form/TextInput.vue';
import Button from './Form/Button.vue';
const fullName = ref('');
const email = ref('');
const githubUsername = ref('');
</script>

<template>
    <div class="w-[60%] max-w-[480px] h-full flex flex-col items-center justify-start gap-5">
        <ElementWithTitle title="Upload Avatar">
            <ImageInput />
        </ElementWithTitle>
        <ElementWithTitle title="Full Name">
            <TextInput v-model="fullName" />
        </ElementWithTitle>
        <ElementWithTitle title="Email">
            <TextInput v-model="email" placeholder="example@email.com" />
        </ElementWithTitle>
        <ElementWithTitle title="Github Username">
            <TextInput v-model="githubUsername" placeholder="@yourusername" textTemplate="@%" hideTemplateWhenEmpty textTemplateOnlyRender />
        </ElementWithTitle>
        <Button>Generate My Ticket</Button>
    </div>
</template>