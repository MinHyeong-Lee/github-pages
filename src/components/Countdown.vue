<template>
  <section class="countdown">
    <p class="label">Wedding Day</p>
    <h2>D-{{ daysLeft }}</h2>

    <div class="times">
      <p lang="ko" class="time">
        미국 서부 시간<br />
        <strong>{{ pacificTime }}</strong>
      </p>

      <p lang="ko" class="time">
        한국 표준시<br />
        <strong>{{ koreaTime }}</strong>
      </p>
    </div>
  </section>
</template>

<script setup>
import { computed } from "vue";

/**
 * 기준 시간
 * 2026-01-27 17:00 (PT, UTC-8)
 * → UTC: 2026-01-28 01:00
 */
const weddingUTC = new Date("2026-01-28T01:00:00Z");

/* D-Day 계산 (날짜 기준) */
const daysLeft = computed(() => {
  const now = new Date();
  const diff = weddingUTC.getTime() - now.getTime();
  return Math.max(Math.ceil(diff / (1000 * 60 * 60 * 24)), 0);
});

/* 미국 서부시간 표시 */
const pacificTime = computed(() =>
  weddingUTC.toLocaleString("en-US", {
    timeZone: "America/Los_Angeles",
    dateStyle: "long",
    timeStyle: "short",
  }),
);

/* 한국 시간 표시 */
const koreaTime = computed(() =>
  weddingUTC.toLocaleString("ko-KR", {
    timeZone: "Asia/Seoul",
    dateStyle: "long",
    timeStyle: "short",
  }),
);
</script>

<style scoped>
.countdown {
  text-align: center;
  padding: 48px 0;
}

.label {
  font-size: 14px;
  letter-spacing: 0.12em;
  color: #777;
}

h2 {
  font-size: 36px;
  margin: 12px 0 24px;
}

.times {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.time {
  font-size: 14px;
  color: #555;
  line-height: 1.4;
}

.time strong {
  font-size: 15px;
  color: #222;
}
</style>
