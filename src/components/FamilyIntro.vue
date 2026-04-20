<template>
  <section class="family-section">
    <!-- 신랑 -->
    <div class="family-block">
      <p lang="ko" class="parents">
        <span class="deceased"><FlowerIcon /></span>이강재 · 최경애 의 아들
      </p>
      <p class="person">
        <span lang="ko" class="label">신랑</span>
        <span lang="ko" class="name">민형</span>
        <span class="eng">MINHYEONG</span>
      </p>
    </div>

    <div class="divider"></div>

    <!-- 신부 -->
    <div class="family-block">
      <p lang="ko" class="parents">최동기 · 정민숙 의 딸</p>
      <p class="person">
        <span lang="ko" class="label">신부</span>
        <span lang="ko" class="name">현정</span>
        <span class="eng">HYEONJEONG</span>
      </p>
    </div>
  </section>
</template>

<script setup>
import FlowerIcon from "@/components/icons/FlowerIcon.vue";
</script>

<style scoped>
.family-section {
  text-align: center;
  color: #222;
  padding: 40px 0;
  /* background: #f4f4f4; */
  background: #fdfbf8;
}

.family-block {
  padding: 16px 0;
}

.parents {
  font-size: 14px;
  color: #666;
  margin-bottom: 10px;
}

.person {
  font-size: 18px;
  letter-spacing: 0.02em;
}

.label {
  font-size: 14px;
  color: #888;
  margin-right: 6px;
}

.name {
  font-weight: 500;
  margin-right: 4px;
}

.eng {
  font-size: 13px;
  letter-spacing: 0.18em;
  color: #555;
}

.divider {
  width: 60%;
  height: 1px;
  /* background: #d5d5d5; */
  background: #e0dcd9;
  margin: 32px auto;
}

.deceased {
  display: inline-flex;
  vertical-align: middle;
  margin: 0 2px;
}
</style>
