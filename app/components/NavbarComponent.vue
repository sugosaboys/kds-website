<script setup lang="ts">
interface Logo{
    url:string;
    name:string;
}
interface NavLinks{
    label:string;
    navlinks:string;
}
interface subNavbar{
    label:string;
    navlinks:string;
}
interface Navbar{
    logo:Logo;
    navlinks:NavLinks[];
    subNavbar:subNavbar[];
}
defineProps<{
    Navbar:Navbar
}>()
const showSubNavbar = ref(false);
const route = useRoute();
watch(()=> route.hash,(newHash)=>{
    if(newHash === '#orderNow'){
        showSubNavbar.value = true;
    }else{
        showSubNavbar.value = false;
    }
},{immediate:true})

const isToggle = ref(true);
const toggleMenu = () => {
    isToggle.value = !isToggle.value;
}
</script>

<template>
    <header class="fixed top-0 w-full z-100 bg-[#f6efe5]">
        <div class="flex justify-between items-center px-8 md:px-24">
            <nuxt-link to="/">
                <NuxtImg v-if="Navbar.logo.url" :src="Navbar?.logo.url" :alt="Navbar?.logo.name" class="w-[80px]"/>
            </nuxt-link>
            <button @click="toggleMenu" class="visible md:hidden">
                <NuxtImg src="/hamburger.png"/>
            </button>
            <nav :class="['transition-all duration-300 fixed top-[81px] left-0 md:top-0 md:left-0 md:relative bg-[#f6efe5] md:bg-transparent w-full md:w-auto h-full md:h-auto flex flex-col md:flex-row justify-center items-center gap-5 md:opacity-100 md:pointer-events-auto',isToggle ? 'opacity-0 pointer-events-none':'opactity-100 pointer-events-auto']">
                <span v-for="Links in Navbar?.navlinks">
                    <a :href="Links.navlinks" class="heebo font-bold text-[14px] relative inline-block after:transition-all after:duration-300 after:absolute after:content-[''] after:bg-black after:bottom-0 after:left-0 after:w-0 after:h-[0.5px] hover:after:w-full">{{ Links.label }}</a>
                </span>
            </nav>
            <div :class="['absolute top-15 right-14 transition-all duration-300 ', showSubNavbar ? 'opacity-100 z-10 pointer-events-auto' : 'opacity-0 pointer-events-none']">
                <main class="flex flex-col gap-3 bg-white rounded-sm w-[120px] p-3">
                    <span v-for="subNavbar in Navbar.subNavbar">
                        <a :href="subNavbar.navlinks" class="heebo font-bold text-[14px]">{{ subNavbar.label }}</a>
                    </span>
                </main>
            </div>
        </div>
    </header>
</template>