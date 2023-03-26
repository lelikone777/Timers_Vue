<template>
  <ul class="list">
    <li class="card" v-for="(timer, index) in timers" :key="index">
      <p class="timer">{{ formatTime(timer.currentTime, timer.currentHours) }}</p>
      <div class="btn-group">
        <button class="play btn" v-if="!timer.isRunning" @click="startTimer(index)"><svg width="17" height="20" viewBox="0 0 17 20" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M0 20V0L17 10L0 20Z" fill="#9E9E9E"/>
        </svg></button>
        <button class="pause btn" v-if="timer.isRunning" @click="stopTimer(index)"><svg width="10" height="20" viewBox="0 0 10 20" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect x="7" width="3" height="20" fill="#9E9E9E"/>
          <rect width="3" height="20" fill="#9E9E9E"/>
        </svg>
        </button>
        <button class="stop btn" @click="resetTimer(index)"><svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect width="20" height="20" fill="#9E9E9E"/>
        </svg>
        </button>
      </div>
    </li>
    <button class="btn-add" @click="addTimer"><svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
      <rect x="8.5" width="3" height="20" fill="#9E9E9E"/>
      <rect y="11.5" width="3" height="20" transform="rotate(-90 0 11.5)" fill="#9E9E9E"/>
    </svg>
    </button>
  </ul>
</template>

<style scoped>
  .list {
    display: flex;
    flex-wrap: wrap;
    row-gap: 50px;
    column-gap: 45px;
    padding: 0 17vw;
    margin: 72px auto 0;
  }
  .card, .btn-add {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 225px;
    height: 120px;
    background-color: #696969;
    color: #9E9E9E;
    font-size: 30px;
    line-height: 1.2;
  }
  .timer {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50%;
    width: 100%;
    border-bottom: 1px solid #9E9E9E;
    font-style: normal;
    font-weight: 400;
    font-size: 22px;
  }
  .btn-group {
    display: flex;
    height: 50%;
    width: 100%;
    align-items: center;
    justify-content: center;
  }
  .btn {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 20px 26px;
    width: 70px;
    background-color: transparent;
  }
  .btn-add {
    display: flex;
    align-items: center;
    justify-content: center;
  }
  @media (max-width: 768px) {
    .list {
      max-width: 225px;
      padding: 0;
    }
  }

</style>

<script>
export default {
  data() {
    return {
      timers: [
        {
          currentTime: 0,
          currentHours: 0,
          isRunning: false,
          interval: null,
        },
      ],
    };
  },
  methods: {
    startTimer(index) {
      this.timers[index].isRunning = true;
      this.timers[index].interval = setInterval(() => {
        this.timers[index].currentTime += 1;
        if (this.timers[index].currentTime === 3600) {
          this.timers[index].currentHours += 1;
          this.timers[index].currentTime = 0;
        }
      }, 1000);
    },
    stopTimer(index) {
      this.timers[index].isRunning = false;
      clearInterval(this.timers[index].interval);
    },
    resetTimer(index) {
      this.timers[index].isRunning = false;
      clearInterval(this.timers[index].interval);
      this.timers[index].currentTime = 0;
      this.timers[index].currentHours = 0;
    },
    addTimer() {
      this.timers.push({
        currentTime: 0,
        currentHours: 0,
        isRunning: false,
        interval: null,
      });
    },
    formatTime(seconds, hours) {
      const h = hours > 0 ? (hours < 10 ? "0" + hours : hours) + ":" : "";
      const m = Math.floor(seconds / 60);
      const s = seconds % 60;
      const formattedM = m > 0 ? (m < 10 ? "0" + m : m) + ":" : "";
      const formattedS = s < 10 ? "0" + s : s;
      return h + formattedM + formattedS;
    },
  },
};
</script>
