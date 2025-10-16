<script setup lang="ts">
const contentFetching = useRuntimeConfig().public.CONTENT;
interface HeroSection{
    __component:'hero.theme1'
    HeroImage:{
        url:string;
        name:string;
    }
    HeroText:string
}

interface sliderTheme{
    __component:"carousels.slider-theme"
    Title:string;
    description:string;
    image:{
        url:string;
        name:string;
    }[]
}

interface leftTheme{
    __component:'contentwith-media.left-theme'
    Title:string;
    description:string;
    image:{
        url:string;
        name:string;
    }
}

interface MenuTheme{
    __component:'menu.menu-theme1'
    Title:string;
    description:string;
    ListMenu:{
        url:string;
        name:string;
    }
}

interface SEO {
    MetaTitle:string;
    MetaDescription:string;
    MetaKeywords:string;
}

type ContentSection = HeroSection | leftTheme | MenuTheme | sliderTheme;

interface HomepageResponse {
    data: {
        navbar: any;
        content:ContentSection[];
        Footer:any;
        seo:SEO;
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
        'populate[seo][populate]':'*',
         
    }
});
const Navbar = computed(()=> data.value?.data.navbar);
const subNavbar = computed(()=> Navbar.value?.subNavbar);
const content = computed(() => data.value?.data.content);
const Footer = computed(()=> data.value?.data.Footer);

const currentSlide = ref(0);
const dotCount = ref(0);

const [sliderRef,slider] = useKeenSlider({
    slides:{
        perView:2,
        spacing:24,
    },
    mode:'free',
    loop:false,
    created(s){
        dotCount.value = s.track.details.slides.length;
    },
    slideChanged(s){
        currentSlide.value = s.track.details.rel;
    },
});

const goToSlide = (idx:number) => {
    if(slider.value){
        slider.value.moveToIdx(idx);
    }
}

const nextSlide = () => slider.value?.next();
const prevSlide = () => slider.value?.prev();

useHead({
    title:data.value?.data.seo.MetaTitle || '',
    meta:[
        {name:'description',content:data.value?.data.seo.MetaDescription || ''},
        {name:'keywords',content:data.value?.data.seo.MetaKeywords || ''},
    ],
    link:[{rel:'icon', type:'image/png', href:'/kopi-deket-sini.png'}]
})
</script>


<template>
 <NavbarComponent v-if="Navbar" :subNavbar="subNavbar" :Navbar="Navbar"/>
 <div v-for="(section,index) in content" :key="index">
    <!--Hero-->
    <section v-if="section.__component === 'hero.theme1'" class="relative flex justify-center items-center">
        <NuxtImg v-if="section.HeroImage?.url" :src="section.HeroImage.url" :alt="section.HeroImage.name" class="w-full h-full md:h-[830px] object-cover brightness-40"/>
        <h1 class="absolute text-[#f0e4d3] text-[32px] md:text-[48px] text-center heebo font-bold">{{ section.HeroText }}</h1>
    </section>
     <!--Carousel-->
    <section id="about" v-if="section.__component === 'carousels.slider-theme'">
        <span class="flex flex-col justify-center items-center gap-8 pt-40">
            <h4 class="text-[20px] heebo font-bold text-center">{{section.Title}}</h4>
            <p class="heebo text-center w-auto md:w-[640px]">{{ section.description }}</p>
        </span>
        <main class="flex justify-center pt-14 pb-10">
            <div ref="sliderRef" class="keen-slider h-[490px]">
                <div v-for="(item, i) in section.image" :key="i" class="keen-slider__slide number-slide1">
                    <NuxtImg :src="item.url" class="w-[736px] h-full object-cover"/>
                </div>
            </div>
        </main>
        <div class="flex justify-center items-center pb-40">
            <span class="flex justify-between items-center w-[300px]">
                <svg width="30" height="30" viewBox="0 0 10 16" @click="prevSlide" class="hover:bg-[#f0e4d3] p-1 cursor-pointer" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M7.75697 16L9.5 14.3134L2.9664 8L9.5 1.6866L7.75203 0L1.22337 6.3134C0.760196 6.76076 0.5 7.36743 0.5 8C0.5 8.63257 0.760196 9.23923 1.22337 9.68659L7.75697 16Z" fill="#6F4E37"/>
                </svg>
                <div class="flex gap-2">
                    <button
                   v-for="i in dotCount"
                   :key="i"
                   @click="goToSlide(i - 1)"
                   class="w-3 h-3 rounded-full transition-all"
                   :class="currentSlide === i - 1 ? 'bg-[#6F4E37] scale-60' : 'bg-[#d6d6d6] scale-60'"
                   ></button>
                </div>
                <svg width="30" height="30" viewBox="0 0 10 16" @click="nextSlide" class="hover:bg-[#f0e4d3] p-1 cursor-pointer" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M2.24303 16L0.5 14.3134L7.0336 8L0.5 1.6866L2.24797 0L8.77663 6.3134C9.2398 6.76076 9.5 7.36743 9.5 8C9.5 8.63257 9.2398 9.23923 8.77663 9.68659L2.24303 16Z" fill="#6F4E37"/>
                </svg>
            </span>
        </div>
    </section>
     <!--content with -->
   <section v-if="section.__component === 'contentwith-media.left-theme'" class="flex flex-col-reverse md:flex-row justify-between items-center">
    <div class="relative flex flex-col justify-center items-center px-20 gap-8 bg-[#dcc5b2] w-full h-[480px]">
        <h4 class="text-[32px] heebo font-bold text-center">{{section.Title}}</h4>
        <p class="heebo text-center">{{ section.description }}</p>
    </div>
    <NuxtImg :src="section.image.url" :alt="section.image.name" class="w-full h-[480px] object-cover"/>
  </section>
   <!--Menu-->
  <section id="menu" v-if="section.__component === 'menu.menu-theme1'" class="flex flex-col justify-center items-center px-30 py-40 gap-8">
    <h4 class="text-[20px] heebo font-bold text-center">{{section.Title}}</h4>
    <p class="heebo text-center w-auto md:w-[640px]">{{ section.description }}</p>
    <NuxtImg :src="section.ListMenu.url" :alt="section.ListMenu.name"/>
 </section>
 </div>
 <FooterComponent :Footer="Footer"/>
</template>