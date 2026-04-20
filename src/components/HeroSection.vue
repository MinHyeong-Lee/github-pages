<template>
  <section class="hero">
    <img
      class="hero-image"
      :src="heroImage"
      alt="Wedding Photo"
      loading="eager"
      decoding="async"
      fetchpriority="high"
    />

    <div class="overlay">
      <h1 class="names">
        Minhyeong<br />
        &<br />
        Hyeonjeong
      </h1>
      <p class="date">January 27, 2026</p>
    </div>
  </section>
</template>

<script setup>
import heroImage from "@/assets/optimized/hero/005.webp";
</script>

<style scoped>
.hero {
  position: relative;
  width: 100%;
}

.hero-image {
  width: 100%;
  height: 70vh;
  object-fit: cover;
  display: block;
}

.overlay {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: rgba(0, 0, 0, 0.25);
  color: white;
  text-align: center;
}

.names {
  font-size: 32px;
  font-weight: 500;
  letter-spacing: 1px;
  line-height: 1.4;
}

.date {
  margin-top: 12px;
  font-size: 14px;
  opacity: 0.9;
}
</style>
