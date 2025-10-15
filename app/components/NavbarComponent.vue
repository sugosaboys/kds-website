<script setup lang="ts">
interface Logo{
    url:string;
    name:string;
}
interface NavLinks{
    label:string;
    navlinks:string;
}
interface Navbar{
    logo:Logo;
    navlinks:NavLinks[];
}
defineProps<{
    Navbar:Navbar
}>()
</script>

<template>
    <header class="bg-[#f6efe5]">
        <div class="flex justify-between items-center px-24">
            <img v-if="Navbar.logo.url" :src="Navbar?.logo.url" :alt="Navbar?.logo.name" class="w-[80px]">
            <nav class="flex items-center gap-5">
                <span v-for="Links in Navbar?.navlinks">
                    <nuxt-link :to="Links.navlinks" class="font-medium relative inline-block after:transition-all after:duration-300 after:absolute after:content-[''] after:bg-black after:bottom-0 after:left-0 after:w-0 after:h-[0.5px] hover:after:w-full">{{ Links.label }}</nuxt-link>
                </span>
            </nav>
        </div>
    </header>
</template>