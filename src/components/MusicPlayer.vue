<template>
  <!-- 배경 클릭 영역 -->
  <div v-if="expanded" class="overlay" @click="expanded = false" />

  <div class="player" :class="{ expanded }">
    <button class="play" @click.stop="toggle" aria-label="music toggle">
      <span v-show="!playing"><PlayIcon /></span>
      <span v-show="playing"><PauseIcon /></span>
    </button>

    <template v-if="expanded">
      <input
        class="progress"
        type="range"
        min="0"
        :max="duration"
        step="0.1"
        v-model="currentTime"
        @input="seek"
      />

      <input
        class="volume"
        type="range"
        min="0"
        max="1"
        step="0.01"
        v-model="volume"
      />
    </template>

    <audio
      ref="audio"
      :src="bgm"
      preload="metadata"
      @timeupdate="updateTime"
      @loadedmetadata="setDuration"
    />
  </div>
</template>

<script setup>
import { ref, watch, onMounted, onUnmounted } from "vue";
import PlayIcon from "./icons/PlayIcon.vue";
import PauseIcon from "./icons/PauseIcon.vue";
import bgm from "@/assets/music/bgm.mp3";

const audio = ref(null);
const playing = ref(false);
const expanded = ref(false);
const currentTime = ref(0);
const duration = ref(0);
const volume = ref(0.1);

// ---------------------------------------
// 1. 초기 사용자 인터랙션 감지 (자동재생용)
// ---------------------------------------
const removeInteractionListeners = () => {
  document.removeEventListener("click", startAudio);
  document.removeEventListener("touchstart", startAudio);
  document.removeEventListener("keydown", startAudio);
};

const startAudio = async () => {
  if (audio.value && !playing.value) {
    try {
      audio.value.volume = volume.value;
      await audio.value.play();

      playing.value = true;
      expanded.value = true; // 재생되면 확장

      removeInteractionListeners();
    } catch (e) {
      // 재생 실패 시 무시
    }
  }
};

// ---------------------------------------
// 2. 스크롤 시 플레이어 축소 (요청하신 기능)
// ---------------------------------------
const handleScrollClose = () => {
  // 스크롤이 발생하면 플레이어를 닫음
  if (expanded.value) {
    expanded.value = false;
  }
};

// expanded 상태를 감시하여 스크롤 리스너 탈부착
watch(expanded, (isExpanded) => {
  if (isExpanded) {
    // 🔥 펼쳐졌을 때: 바로 닫히지 않게 0.5초(500ms) 여유를 둔 뒤 스크롤 감지 시작
    setTimeout(() => {
      // { once: true } 옵션을 써서 한 번 감지되면 리스너가 자동 삭제되도록 함
      window.addEventListener("scroll", handleScrollClose, { once: true });
    }, 800);
  } else {
    // 닫혔을 때: 혹시 남아있을 리스너 제거
    window.removeEventListener("scroll", handleScrollClose);
  }
});

// ---------------------------------------
// 3. 라이프사이클 및 기타 로직
// ---------------------------------------
onMounted(() => {
  document.addEventListener("click", startAudio);
  document.addEventListener("touchstart", startAudio);
  document.addEventListener("keydown", startAudio);
});

onUnmounted(() => {
  removeInteractionListeners();
  window.removeEventListener("scroll", handleScrollClose);
});

const toggle = async () => {
  if (!audio.value) return;
  audio.value.volume = volume.value;

  if (!playing.value) {
    try {
      await audio.value.play();
      expanded.value = true;
      playing.value = true;
      removeInteractionListeners();
    } catch (e) {
      console.error(e);
    }
  } else {
    audio.value.pause();
    playing.value = false;
  }
};

const setDuration = () => {
  if (!audio.value) return;
  duration.value = audio.value.duration;
  audio.value.volume = volume.value;
};

const updateTime = () => {
  if (audio.value) currentTime.value = audio.value.currentTime;
};

const seek = () => {
  if (audio.value) audio.value.currentTime = currentTime.value;
};

// 볼륨 조절 시에는 닫히지 않도록 이벤트 전파 방지는 템플릿의 @click.stop 등으로 처리됨
watch(volume, (v) => {
  if (audio.value) audio.value.volume = v;
});
</script>

<style scoped>
/* -------------------------
   iOS / Android 포커스 제거
------------------------- */
button,
input {
  outline: none;
  box-shadow: none;
  -webkit-tap-highlight-color: transparent;
}

button:focus,
button:focus-visible {
  outline: none;
}

/* -------------------------
   배경 클릭 영역
------------------------- */
.overlay {
  position: fixed;
  inset: 0;
  background: transparent;
  z-index: 10;
}

/* -------------------------
   플레이어 기본
------------------------- */
.player {
  position: fixed;
  bottom: 16px;
  right: 16px;
  z-index: 20;

  display: flex;
  align-items: center;
  gap: 8px;

  height: 36px; /* 🔽 더 작게 */
  width: 36px;

  border-radius: 50%; /* 자연스러운 pill */
  backdrop-filter: blur(12px);
  background: rgba(0, 0, 0, 0.45);

  color: white;
  transition:
    width 0.25s ease,
    padding 0.25s ease,
    border-radius 0.25s ease;
}

/* 확장 상태 */
.player.expanded {
  width: 240px;
  padding-right: 8px;
  border-radius: 18px;
}

/* 축소 상태 */
.player:not(.expanded) {
  width: 38px;
  justify-content: center;
  border-radius: 999px; /* 완전 원형 */
}

/* -------------------------
   재생 버튼
------------------------- */
.play {
  width: 28px;
  height: 28px;

  display: flex;
  align-items: center;
  justify-content: center;

  font-size: 14px;
  background: none;
  border: none;
  color: white;

  cursor: pointer;
}

/* 아이콘 흔들림 방지 */
.play span {
  width: 14px;
  text-align: center;
}

/* -------------------------
   슬라이더
------------------------- */
.progress {
  flex: 1;
}

.volume {
  width: 50px;
}

/* 공통 */
input[type="range"] {
  -webkit-appearance: none;
  appearance: none;
  height: 4px; /* 🔑 핵심: 4px */
  background: transparent;
}

/* track */
input[type="range"]::-webkit-slider-runnable-track {
  height: 4px;
  background: rgba(255, 255, 255, 0.3);
  border-radius: 2px;
}

/* thumb */
input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;

  width: 10px;
  height: 10px;
  margin-top: -3px;

  background: white;
  border-radius: 50%;
  border: none;
  box-shadow: none; /* 🔑 blur 제거 */
}

/* Firefox */
input[type="range"]::-moz-range-track {
  height: 4px;
  background: rgba(255, 255, 255, 0.3);
  border-radius: 2px;
}

input[type="range"]::-moz-range-thumb {
  width: 10px;
  height: 10px;
  background: white;
  border-radius: 50%;
  border: none;
}
</style>
