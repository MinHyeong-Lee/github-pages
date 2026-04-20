<template>
  <section class="about-us">
    <div class="header-text">
      <h2 class="title">ABOUT US</h2>
      <p lang="ko" class="subtitle-main">저희 커플을 소개합니다</p>
      <p lang="ko" class="subtitle-sub">하나로 이어진 두개의 우주</p>
    </div>

    <div ref="storyStage" class="story-stage">
      <div class="story-card-shell">
        <div class="profile-card">
          <div class="image-container">
            <img
              :src="aboutImage"
              :alt="activePerson.name"
              loading="lazy"
              decoding="async"
            />
          </div>

          <Transition name="story-copy" mode="out-in">
            <div :key="activePerson.role" class="info-container">
              <div lang="ko" class="name-tag">
                <span class="role">{{ activePerson.role }}</span>
                <span class="name">{{ activePerson.name }}</span>
              </div>

              <div lang="ko" class="description">
                <p>
                  <span v-if="activePerson.isDeceased" class="deceased">
                    <FlowerIcon />
                  </span>
                  {{ activePerson.parents }}
                </p>
                <p>{{ activePerson.birth }}</p>
                <p>{{ activePerson.motto }}</p>
              </div>
            </div>
          </Transition>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { computed, onBeforeUnmount, onMounted, shallowRef, useTemplateRef } from "vue";
import FlowerIcon from "@/components/icons/FlowerIcon.vue";
import aboutImage from "@/assets/optimized/about/025.webp";

const profileDescriptions = [
  {
    role: "신랑",
    name: "이민형",
    isDeceased: true,
    parents: "이강재 · 최경애의 아들",
    birth: "1994년 1월 서울 출생",
    motto: "현정바라기",
  },
  {
    role: "신부",
    name: "최현정",
    parents: "최동기 · 정민숙의 딸",
    birth: "1991년 3월 광주 출생",
    motto: "민형바라기",
  },
];

const storyStageRef = useTemplateRef("storyStage");
const activeIndex = shallowRef(0);

function clamp(value, min, max) {
  return Math.min(Math.max(value, min), max);
}

function updateActiveIndex() {
  const stage = storyStageRef.value;

  if (!stage) {
    return;
  }

  const rect = stage.getBoundingClientRect();
  const scrollableDistance = Math.max(stage.offsetHeight - window.innerHeight, 1);
  const progress = clamp(-rect.top / scrollableDistance, 0, 1);

  activeIndex.value = progress >= 0.5 ? 1 : 0;
}

onMounted(() => {
  updateActiveIndex();
  window.addEventListener("scroll", updateActiveIndex, { passive: true });
  window.addEventListener("resize", updateActiveIndex);
});

onBeforeUnmount(() => {
  window.removeEventListener("scroll", updateActiveIndex);
  window.removeEventListener("resize", updateActiveIndex);
});

const activePerson = computed(() => profileDescriptions[activeIndex.value]);
</script>

<style scoped>
.about-us {
  background-color: #ffffff;
  padding: 80px 20px;
  text-align: center;
}

.header-text {
  margin-bottom: 40px;
}

.title {
  font-size: 2.5rem;
  font-weight: 300;
  letter-spacing: 4px;
  color: #333;
  margin-bottom: 20px;
}

.subtitle-main {
  font-size: 1.1rem;
  color: #444;
  margin-bottom: 8px;
  font-weight: 500;
}

.subtitle-sub {
  font-size: 0.95rem;
  color: #888;
  letter-spacing: -0.5px;
}

.story-stage {
  min-height: 122vh;
  position: relative;
}

.story-card-shell {
  position: sticky;
  top: 24px;
  display: flex;
  justify-content: center;
}

.profile-card {
  background-color: #ffffff;
  border-radius: 15px;
  padding: 0;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.04);
  border: 1px solid #f0f0f0;
  max-width: 320px;
  margin: 0 auto;
}

.image-container {
  width: 100%;
  aspect-ratio: 4 / 5;
  overflow: hidden;
  border-radius: 12px;
  margin-bottom: 25px;
}

.image-container img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.name-tag {
  font-size: 1.2rem;
  margin-bottom: 20px;
  display: flex;
  justify-content: center;
  gap: 8px;
}

.role {
  color: #888;
  font-weight: 400;
}

.name {
  color: #222;
  font-weight: 600;
}

.description {
  line-height: 1.8;
  color: #555;
  font-size: 0.95rem;
}

.description p {
  margin: 0;
}

.deceased {
  display: inline-flex;
  vertical-align: middle;
  margin: 0 2px;
}

.info-container {
  padding: 24px 20px;
}

.story-copy-enter-active,
.story-copy-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.story-copy-enter-from,
.story-copy-leave-to {
  opacity: 0;
  transform: translateY(10px);
}

@media (max-width: 380px) {
  .story-stage {
    min-height: 128vh;
  }
}
</style>
