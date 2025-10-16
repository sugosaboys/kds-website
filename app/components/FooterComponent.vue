<script setup lang="ts">
interface SocialMedia{
    link:string;
    label:string;
    image:{
        url:string;
        name:string;
    }
}
interface Address{
    navlinks:string;
    label:string;
}
    interface Footer{
        BackgroundImage:{
            url:string;
            name:string;
        }
        SocialMedia:SocialMedia[];
        Address:Address;
        Phone:string;
        FooterBottomURL:string;
    }
    defineProps<{
        Footer:Footer
    }>();
</script>

<template>
   <footer class="relative">
    <NuxtImg :src="Footer.BackgroundImage.url" :alt="Footer.BackgroundImage.name" class="w-full h-[730px] object-cover brightness-40" />
    <div class="absolute inset-0 px-8 md:px-30 py-40 flex flex-col md:flex-row justify-between items-start text-white w-full">
        <h4 class="text-[32px] font-bold">Connect With Us</h4>
        <main class="flex flex-col gap-8">
            <div class="flex gap-10">
                <p class="font-medium w-40">Opening Hours</p>
                <div class="flex flex-col gap-3">
                    <p>Everyday</p>
                    <p>10.00 am - 11.00 pm</p>
                </div>
            </div>
            <div class="flex gap-10">
                <p class="font-medium w-40">Address</p>
                <div class="flex flex-col gap-3">
                    <p class="w-[362px]">{{Footer.Address.label}}</p>
                     <a :href="Footer.Address.navlinks" class="transition-all duration-300 border-1 border-white rounded-lg py-2 px-2 w-[120px] text-center hover:border-[#1a1612] hover:bg-[#1a1612]">Get Directions</a>
                </div>
            </div>
            <div class="flex gap-10">
                <p class="font-medium w-40">Contact</p>
                <p>{{ Footer.Phone }}</p>
            </div>
            <div class="flex gap-10">
                <p class="font-medium w-40">Social Media</p>
                <div class="flex gap-5">
                    <span v-for="socialMedia in Footer.SocialMedia">
                        <a :href="socialMedia.link"><NuxtImg :src="`/socialmedia/${socialMedia.label}-kds.svg`" :alt="socialMedia.label" class="w-[40px]"/></a>
                    </span>
                </div>
            </div>
        </main>
    </div>
    <div class="bg-[#dcc5b2] py-2 text-center">
        <nuxt-link :to="Footer.FooterBottomURL">© 2025 Kopi Deket Sini</nuxt-link>
    </div>
</footer>
</template>