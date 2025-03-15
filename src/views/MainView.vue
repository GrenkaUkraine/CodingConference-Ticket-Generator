<script setup lang="ts">
import MainContainer from '../components/MainContainer.vue';
import LogoFull from '../assets/icons/logo-full.svg';
import FormComponent from '../components/FormComponent.vue';
import TicketComponent from '../components/TicketComponent.vue';
import { ref } from 'vue';

const devModeFormSubmitted = ref(false);
const formSubmitted = ref(false);
const ticketData = ref({
    fullName: '',
    email: '',
    githubUsername: '',
    avatarImage: null as string | null
});

const handleFormSubmit = (formData: {
    fullName: string;
    email: string;
    githubUsername: string;
    avatarImage: string | null;
}) => {
    console.log('Form submitted with data:', formData);
    formSubmitted.value = true;
    ticketData.value = formData;
};
</script>

<template>
    <MainContainer>
        <div class="w-full h-full flex flex-col items-center justify-start py-10 gap-[5vh]">
            <LogoFull />
            <div class="w-[60%] h-full flex flex-col items-center justify-start gap-[2.5vh]">
                <h1 class="text-center">
                    Your Journey to Coding Conf 2025 Starts Here!
                </h1>
                <h2 class="text-center">
                    Secure your spot at next year's biggest coding conference.
                </h2>
            </div>
            <FormComponent v-if="!formSubmitted && !devModeFormSubmitted" @submit="handleFormSubmit" />
            <TicketComponent v-else :ticket-data="ticketData" />
        </div>
    </MainContainer>
</template>
