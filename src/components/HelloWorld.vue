<template>
  <div class="hello">
    <div class="section">
      <div v-if="!isSkipped" class="pub">
        <div v-if="!isPlaying" class="blur"></div>
        <video class="video" ref="videoRef">
          <source :src="`/assets/videos/${videoNum}.mp4`" type="video/mp4">
          Your browser does not support the video tag.
        </video>
        <button v-if="!isPlaying" class="play" @click="play"><img src="@/assets/img/play.png" alt="play-btn"></button>
      </div>
      <div v-else>
        <TextItem></TextItem>
      </div>
    </div>
    <button v-if="!isSkipped" class="skip" @click="skip">Passer les annonces ></button>
  </div>
</template>

<script>
import { onMounted, onBeforeUnmount, ref } from 'vue';
import TextItem from '@/components/TextItem.vue';
export default {
  name: 'HelloWorld',
  components: { TextItem },
  setup() {
    const isPlaying = ref(false);
    const isSkipped = ref(false);

    const play = () => {
      isPlaying.value = true;
      const video = document.querySelector('video');
      video.play();
    };

    const skip = () => {
      isSkipped.value = true;
    };

    const keydownStuff = (event) => {
      if (event.code === 'Enter' || event.code === 'Space') {
        play();
      }
    };

    onMounted(() => {
      window.addEventListener('keydown', keydownStuff);
    });

    onBeforeUnmount(() => {
      window.removeEventListener('keydown', keydownStuff);
    });

    const getRandomInt = (max) => {
      return Math.floor(Math.random() * max);
    }

    const videoNum = ref(getRandomInt(6) + 1);
    return {
      videoNum,
      isPlaying,
      isSkipped,
      play,
      skip,
    }
  },
}
</script>

<style scoped>
.hello {
  position: absolute;
  height: 100%;
  width: 100%;
  box-sizing: border-box;
  top: 0;
  left: 0;
}

.section {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  background: black;
}

.pub video {
  width: 30%;
  border-radius: 8px;
}

button {
  background-color: transparent;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

button.play {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

button.play img {
  width: 75px;
  height: 75px;
}

button.skip {
  background-color: #4CAF50;
  color: white;
  position: fixed;
  padding: 1rem 2rem;
  right: 1.25rem;
  bottom: 1.25rem;
  font-size: 20px;
}

.blur {
  width: 30%;
  height: 100%;
  backdrop-filter: blur(2px);
  position: absolute;
  left: 50%;
  top: 0;
  transform: translate(-50%, 0);
}

@media (max-width: 768px) {
  .pub video {
    width: 100%;
  }

  .blur {
    width: 100%;
  }
}
</style>
