<script setup lang="ts">
import PatternTicket from '../assets/images/pattern-ticket.svg';
import LogoMark from '../assets/icons/logo-mark.svg';
import GithubLogo from '../assets/icons/icon-github.svg';

interface TicketData {
    fullName: string;
    email: string;
    githubUsername: string;
    avatarImage: string | null;
}

defineProps<{
    ticketData: TicketData;
}>();
</script>

<template>
    <div class="relative">
        <div class="ticket-background">
            <PatternTicket />
        </div>

        <div class="absolute inset-0 flex p-6 pl-9 z-10 justify-between items-center">
            <div class="flex flex-col justify-between h-full">
                <div class="flex flex-col gap-3">
                    <div class="flex gap-5 pr-9">
                        <LogoMark />
                        <p class="text-4xl font-black text-text-primary tracking-[-0.8px]">Coding Conf</p>
                    </div>
                    <div class="w-full flex gap-3.5 text-xl text-text-secondary tracking-[-1px] justify-end">
                        <p>Jan 31, 2025</p>
                        <p>/</p>
                        <p>Austin, TX</p>
                    </div>
                </div>
                <div class="flex gap-3.5">
                    <div class="w-20 h-20 rounded-xl bg-cover bg-center"
                        :style="{ backgroundImage: `url(${ticketData.avatarImage})` }"></div>
                    <div class="flex flex-col gap-2">
                        <p class="text-3xl font-medium text-text-primary tracking-[-1.92px]">{{ ticketData.fullName }}</p>
                        <div class="flex gap-2 items-center">
                            <GithubLogo />
                            <p class="text-xl text-text-secondary tracking-[-0.4px]">{{ ticketData.githubUsername }}</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="relative w-12 h-32 flex items-center justify-center">
                <p
                    class="absolute text-[1.875rem] font-medium opacity-35 tracking-[-1.6px] transform rotate-90 origin-center whitespace-nowrap text-text-secondary">
                    #{{ String(Math.floor(Math.random() * 99999) + 1).padStart(5, '0') }}</p>
            </div>
        </div>
    </div>
</template>

<style scoped>
.ticket-background {
    width: auto;
    height: auto;
}
</style>