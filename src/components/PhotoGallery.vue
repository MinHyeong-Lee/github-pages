<template>
  <section class="gallery-wrapper">
    <!-- 갤러리 소제목 -->
    <div class="gallery-header">
      <h2 class="gallery-title">GALLERY</h2>
      <p lang="ko" class="gallery-desc">
        사진을 클릭하시면 전체 화면으로 보실 수 있습니다
      </p>
    </div>
    <!-- 
      가로 스크롤 영역 
      @scroll 이벤트로 현재 페이지 위치를 계산합니다.
    -->
    <div class="slider-container" ref="sliderRef" @scroll="handleScroll">
      <!-- 4장씩 묶인 페이지 (슬라이드) -->
      <div
        v-for="(page, pageIndex) in pagedImages"
        :key="page[0]?.id ?? `page-${pageIndex}`"
        class="slide-page"
      >
        <div class="grid-2x2">
          <a
            v-for="img in page"
            :key="img.id"
            :href="img.lightboxSrc"
            :data-pswp-width="img.lightboxWidth"
            :data-pswp-height="img.lightboxHeight"
            class="frame"
            target="_blank"
            rel="noreferrer"
          >
            <img
              :src="img.thumbSrc"
              :alt="`Wedding photo ${img.id}`"
              loading="lazy"
              decoding="async"
              :width="img.thumbWidth"
              :height="img.thumbHeight"
            />
          </a>
        </div>
      </div>
    </div>

    <!-- 하단 페이지네이션 (점) -->
    <div class="pagination" v-if="pagedImages.length > 1">
      <span
        v-for="(_, index) in pagedImages"
        :key="index"
        class="dot"
        :class="{ active: currentPage === index }"
        @click="scrollToPage(index)"
      ></span>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from "vue";
import PhotoSwipeLightbox from "photoswipe/lightbox";
import "photoswipe/style.css";
import { galleryImages } from "@/data/galleryImages";

const images = galleryImages;

// 4장씩 데이터 분할
const PAGE_SIZE = 4;
const pagedImages = computed(() => {
  const pages = [];
  for (let i = 0; i < images.length; i += PAGE_SIZE) {
    pages.push(images.slice(i, i + PAGE_SIZE));
  }
  return pages;
});

const currentPage = ref(0);
const sliderRef = ref(null);
let lightbox = null;

// 스크롤 이벤트 핸들러: 현재 몇 번째 페이지인지 계산
const handleScroll = () => {
  if (!sliderRef.value) return;
  const scrollLeft = sliderRef.value.scrollLeft;
  const width = sliderRef.value.clientWidth;

  // 절반 이상 넘어가면 페이지 변경 인식
  currentPage.value = Math.round(scrollLeft / width);
};

// 점 클릭 시 해당 페이지로 스크롤 이동
const scrollToPage = (index) => {
  if (!sliderRef.value) return;
  const width = sliderRef.value.clientWidth;
  sliderRef.value.scrollTo({
    left: width * index,
    behavior: "smooth",
  });
};

onMounted(() => {
  // slider-container 내부의 모든 'a' 태그를 대상으로 PhotoSwipe를 초기화합니다.
  lightbox = new PhotoSwipeLightbox({
    gallery: sliderRef.value,
    children: "a",
    pswpModule: () => import("photoswipe"),
    bgOpacity: 0.95,
    showHideAnimationType: "zoom",
  });

  lightbox.init();
});

onBeforeUnmount(() => {
  if (lightbox) {
    lightbox.destroy();
    lightbox = null;
  }
});
</script>

<style>
/* -------------------------
   갤러리 헤더
------------------------- */
.gallery-header {
  text-align: center;
  margin: 64px 0 40px;
}

.gallery-title {
  font-size: 28px;
  font-weight: 400;
  letter-spacing: 0.18em;
  color: #222;
  margin-bottom: 12px;
}

.gallery-desc {
  font-size: 14px;
  color: #888;
  line-height: 1.6;
}

.gallery-wrapper {
  position: relative;
  width: 100%;
  padding: 20px 0;
  /* margin: 24px 0; */
}

/* -------------------------
   가로 스크롤 컨테이너 (Snap 적용)
------------------------- */
.slider-container {
  display: flex;
  overflow-x: auto;
  overflow-y: hidden;
  scroll-snap-type: x mandatory; /* 핵심: 가로 스냅 */

  /* 스크롤바 숨김 */
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none; /* Firefox */
}
.slider-container::-webkit-scrollbar {
  display: none; /* Chrome, Safari */
}

/* -------------------------
   페이지 (슬라이드 단위)
------------------------- */
.slide-page {
  min-width: 100%; /* 부모 너비만큼 꽉 차게 */
  padding: 0 4px; /* 좌우 여백 살짝 */
  box-sizing: border-box;
  scroll-snap-align: center; /* 스크롤 시 중앙 정렬 */
}

/* -------------------------
   내부 2x2 그리드
------------------------- */
.grid-2x2 {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 12px;
}

/* -------------------------
   액자 프레임
------------------------- */
.frame {
  position: relative;
  display: block;
  overflow: hidden;
  border-radius: 8px; /* 둥근 모서리 */
  background: #f3f3f3;
  width: 100%;
  -webkit-tap-highlight-color: transparent;

  /* 포커스 시 파란색 테두리 제거 */
  outline: none;

  /* 꾹 눌렀을 때 이미지 선택/복사 메뉴 뜨는 것 방지 (선택사항) */
  user-select: none;
  -webkit-touch-callout: none;
}

/* 포커스 상태에서도 아웃라인 제거 */
.frame:focus,
.frame:active {
  outline: none;
}

/* 비율 유지 (4:5 비율 예시 - padding-top 조절 가능) */
.frame::before {
  content: "";
  display: block;
  padding-top: 135%;
}

.frame img {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.frame:active img {
  transform: scale(0.96);
}

/* -------------------------
   페이지네이션 (점)
------------------------- */
.pagination {
  display: flex;
  justify-content: center;
  margin-top: 16px;
  gap: 8px;
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: #ddd;
  transition: background-color 0.3s;
  cursor: pointer;
}

.dot.active {
  background-color: #333; /* 활성 색상 */
}

.pswp__button,
.pswp__button:focus,
.pswp__button:active {
  outline: none !important;
  box-shadow: none !important;
  -webkit-tap-highlight-color: transparent;
  tap-highlight-color: transparent;
}

/* 아이콘 SVG 클릭 시도 포함 */
.pswp__button svg {
  pointer-events: none;
}
</style>
