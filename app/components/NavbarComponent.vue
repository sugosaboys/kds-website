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
            <img :src="Navbar?.logo.url" :alt="Navbar?.logo.name" class="w-[80px]">
            <nav class="flex items-center gap-5">
                <span v-for="Links in Navbar?.navlinks">
                    <nuxt-link :to="Links.navlinks" class="font-bold">{{ Links.label }}</nuxt-link>
                </span>
            </nav>
        </div>
    </header>
</template>