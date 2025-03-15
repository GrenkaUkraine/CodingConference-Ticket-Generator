<script setup lang="ts">
import MainContainer from '../components/MainContainer.vue';
import LogoFull from '../assets/icons/logo-full.svg';
import FormComponent from '../components/FormComponent.vue';
import TicketComponent from '../components/TicketComponent.vue';
import { ref, onMounted } from 'vue';

const devModeFormSubmitted = ref(false);
const formSubmitted = ref(false);
const ticketData = ref({
    fullName: '',
    email: '',
    githubUsername: '',
    avatarImage: null as string | null
});

const pageLoaded = ref(false);
const ticketVisible = ref(false);

onMounted(() => {
    setTimeout(() => {
        pageLoaded.value = true;
    }, 100);
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
    
    setTimeout(() => {
        ticketVisible.value = true;
    }, 100);
};
</script>

<template>
    <MainContainer>
        <div class="w-full h-full flex flex-col items-center justify-start py-10 gap-[5vh]"
             :class="{ 'page-loaded': pageLoaded }">
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
            <div v-else class="ticket-container" :class="{ 'ticket-visible': ticketVisible }">
                <TicketComponent :ticket-data="ticketData" />
            </div>
        </div>
    </MainContainer>
</template>

<style scoped>
.w-full {
    opacity: 0;
    transform: translateY(20px);
    transition: all 0.5s ease-in-out;
}

.page-loaded {
    opacity: 1;
    transform: translateY(0);
}

.ticket-container {
    opacity: 0;
    transform: translateY(20px);
    transition: all 0.5s ease-in-out;
    width: 100%;
    display: flex;
    justify-content: center;
}

.ticket-visible {
    opacity: 1;
    transform: translateY(0);
}
</style>
