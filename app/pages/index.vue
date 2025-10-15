<script setup lang="ts">
const contentFetching = useRuntimeConfig().public.CONTENT;
interface HeroSection{
    _component:'hero.theme1'
    HeroImage:any
    HeroText:string
}

type ContentSection = HeroSection;

interface HomepageResponse {
    data: {
        navbar: any;
        content:ContentSection[];
    };
}

const {data} = await useFetch<HomepageResponse>(`${contentFetching}/api/homepage`,{
    server:true,
    lazy:false,
    key:'homepage',
    query:{
        'populate[navbar][populate]':'*',
        'populate[content][populate]':'*',
        'populate[Footer][populate]':'*',
    }
});
const Navbar = computed(()=> data.value?.data.navbar);
const content = computed(() => data.value?.data.content);
console.log(data.value)
</script>


<template>
 <NavbarComponent v-if="Navbar" :Navbar="Navbar"/>
   <!-- <div v-for="heroSection in content">
        <h4>{{ heroSection.HeroText }}</h4>
   </div> -->
</template>