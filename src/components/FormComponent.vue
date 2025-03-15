<script setup lang="ts">
import { ref } from 'vue';
import ElementWithTitle from './Form/ElementWithTitle.vue';
import ImageInput from './Form/ImageInput.vue';
import TextInput from './Form/TextInput.vue';
import Button from './Form/Button.vue';

const fullName = ref('');
const email = ref('');
const githubUsername = ref('');
const avatarImage = ref<string | null>(null);
const formSubmitted = ref(false);

const fullNameError = ref(false);
const emailError = ref(false);
const githubUsernameError = ref(false);
const avatarImageError = ref(false);

const validateEmail = (email: string): boolean => {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(email);
};

const validateForm = (): boolean => {
    let isValid = true;
    
    if (!avatarImage.value) {
        avatarImageError.value = true;
        isValid = false;
    } else {
        avatarImageError.value = false;
    }
    
    if (fullName.value.trim() === '') {
        fullNameError.value = true;
        isValid = false;
    } else {
        fullNameError.value = false;
    }
    
    if (email.value.trim() === '') {
        emailError.value = true;
        isValid = false;
    } else if (!validateEmail(email.value)) {
        emailError.value = true;
        isValid = false;
    } else {
        emailError.value = false;
    }
    
    if (githubUsername.value.trim() === '') {
        githubUsernameError.value = true;
        isValid = false;
    } else {
        githubUsernameError.value = false;
    }
    
    return isValid;
};

const handleSubmit = () => {
    formSubmitted.value = true;
    
    if (!validateForm()) {
        return;
    }

    // TODO: Add form submission logic here
};
</script>

<template>
    <div class="w-[60%] max-w-[480px] h-full flex flex-col items-center justify-start gap-5">
        <ElementWithTitle title="Upload Avatar">
            <ImageInput 
                v-model="avatarImage" 
                :error="formSubmitted && avatarImageError"
                errorMessage="Please upload a photo"
            />
        </ElementWithTitle>
        <ElementWithTitle title="Full Name">
            <TextInput 
                v-model="fullName" 
                :error="formSubmitted && fullNameError"
                errorMessage="Please enter your full name"
            />
        </ElementWithTitle>
        <ElementWithTitle title="Email">
            <TextInput 
                v-model="email" 
                placeholder="example@email.com" 
                :error="formSubmitted && emailError"
                errorMessage="Please enter a valid email address"
            />
        </ElementWithTitle>
        <ElementWithTitle title="Github Username">
            <TextInput 
                v-model="githubUsername" 
                placeholder="@yourusername" 
                textTemplate="@%" 
                hideTemplateWhenEmpty 
                textTemplateOnlyRender 
                :error="formSubmitted && githubUsernameError"
                errorMessage="Please enter your GitHub username"
            />
        </ElementWithTitle>
        <Button @click="handleSubmit">Generate My Ticket</Button>
    </div>
</template>