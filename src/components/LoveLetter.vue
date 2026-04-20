<template>
  <section lang="ko" class="letter-section">
    <h2 class="title">서로에게 쓰는 편지</h2>

    <div class="letter-card">
      <p class="from">신랑이 신부에게</p>

      <p class="content" :class="{ collapsed: !showGroom }">
        {{ groomLetter }}
      </p>

      <button
        v-if="groomLetter.length > limit"
        class="more"
        @click="showGroom = !showGroom"
      >
        {{ showGroom ? "접기" : "더보기" }}
      </button>
    </div>

    <div class="letter-card">
      <p class="from">신부가 신랑에게</p>

      <p class="content" :class="{ collapsed: !showBride }">
        {{ brideLetter }}
      </p>

      <button
        v-if="brideLetter.length > limit"
        class="more"
        @click="showBride = !showBride"
      >
        {{ showBride ? "접기" : "더보기" }}
      </button>
    </div>
  </section>
</template>

<script setup>
import { ref } from "vue";

/**
 * 나중에 이 부분만 교체하면 됨
 */
const groomLetter = `
처음 만났던 날부터 지금까지,
당연하지 않았던 모든 순간들이
당신 덕분에 특별해졌습니다.

앞으로도 기쁜 날엔 함께 웃고,
힘든 날엔 가장 먼저 손을 내밀 수 있는
사람이 되겠습니다.
`;

const brideLetter = `
늘 한결같이 곁을 지켜줘서 고마워요.
당신과 함께라면 어떤 날도
두렵지 않다는 걸 알게 됐어요.

서로에게 가장 편안한 사람이 되어
평범한 하루도 소중히 안고 살아가요.
`;

const limit = 180;
const showGroom = ref(false);
const showBride = ref(false);
</script>

<style scoped>
.letter-section {
  padding: 48px 20px;
  /* background: #fdfbf8; */
  background: #fff1f3;
}

.title {
  text-align: center;
  font-size: 20px;
  font-weight: 600;
  margin-bottom: 32px;
}

.letter-card {
  background: #fff; /* 카드 배경은 흰색 유지 */
  /* 테두리를 살짝 주어 종이 느낌 내기 */
  border: 1px solid rgba(255, 200, 200, 0.2);
  box-shadow: 0 4px 12px rgba(255, 182, 193, 0.15);
}

.from {
  font-size: 14px;
  font-weight: 600;
  margin-bottom: 12px;
  color: #555;
}

.content {
  font-size: 15px;
  line-height: 1.8;
  color: #333;
  white-space: pre-line;
  transition: max-height 0.3s ease;
}

.content.collapsed {
  max-height: 120px;
  overflow: hidden;
}

.more {
  margin-top: 12px;
  background: none;
  border: none;
  font-size: 13px;
  color: #999;
  padding: 0;
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
}

.more:focus,
.more:active {
  outline: none;
}
</style>
