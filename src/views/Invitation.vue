<template>
  <div class="invitation">
    <HeroSection />
    <Countdown />
    <FamilyIntro />
    <AboutUs />
    <LoveLetter />
    <PhotoGallery />
    <WeddingLocation />
    <MusicPlayer />
    <Footer />
  </div>
</template>

<script setup>
import HeroSection from "@/components/HeroSection.vue";
import Countdown from "@/components/Countdown.vue";
import FamilyIntro from "@/components/FamilyIntro.vue";
import AboutUs from "@/components/AboutUs.vue";
import PhotoGallery from "@/components/PhotoGallery.vue";
import WeddingLocation from "@/components/WeddingLocation.vue";
import MusicPlayer from "@/components/MusicPlayer.vue";
import LoveLetter from "@/components/LoveLetter.vue";
import Footer from "@/components/Footer.vue";
</script>

<style scoped>
.invitation {
  max-width: 420px;
  margin: 0 auto;
}
</style>
