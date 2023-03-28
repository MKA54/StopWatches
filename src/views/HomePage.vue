<template>
  <div class="wrapper">
    <div class="wrapper_grid">
      <div v-for="(item, index) in watches" :key="item.id">
        <div class="watch">
          <p class="time" v-bind:class="{ time_text_color_off_watch: item.isOff }">
            <span v-if="item.hours !== null">{{ item.hours }}:</span>
            <span v-if="item.minutes !== null">{{ item.minutes }}:</span>
            <span>{{ item.seconds }}</span>
          </p>
          <hr class="line" v-bind:class="{ line_off_watch: item.isOff, line_on_watch: !item.isOff }"/>
          <p class="watch_buttons">
            <button v-if="item.isOff" @click="start(index)" class="play_button"></button>
            <button v-if="!item.isOff" @click="stop(index)" class="stop_button"></button>
            <button @click="reset(index)" class="reset_button"
                    v-bind:class="{ reset_button_off_watch: item.isOff, reset_button_on_watch: !item.isOff }"></button>
          </p>
        </div>
      </div>
      <div>
        <button @click="addWatch" class="add_button"></button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
  data () {
    return {
      watches: [
        {
          hours: null,
          minutes: null,
          seconds: 0,
          isOff: true
        },
        {
          hours: null,
          minutes: null,
          seconds: 0,
          isOff: true
        }]
    }
  },
  methods: {
    addWatch () {
      this.watches.push({
        hours: null,
        minutes: null,
        seconds: 0,
        isOff: true
      })
    },
    start (index) {
      this.watches[index].watchStatus = 'Включеный счётчик'
      this.watches[index].isOff = false

      const setIntervalId = setInterval(tick, 1000)

      const self = this

      function tick () {
        if (self.watches[index].isOff) {
          clearInterval(setIntervalId)
          return
        }

        self.watches[index].seconds++

        if (self.watches[index].seconds >= 60) {
          self.watches[index].minutes++
          self.watches[index].seconds = 0
        }

        if (self.watches[index].minutes >= 60) {
          self.watches[index].hours++
          self.watches[index].minutes = 0
        }

        if (self.watches[index].hours >= 24) {
          this.stop(index)
        }
      }
    },
    stop (index) {
      this.watches[index].watchStatus = 'Выключеный счётчик'
      this.watches[index].isOff = true
    },
    reset (index) {
      this.watches[index].hours = null
      this.watches[index].minutes = null
      this.watches[index].seconds = 0
    }
  }
}
</script>

<style>
.wrapper {
  margin-top: 72px;
  text-align: center;
}

.wrapper_grid {
  display: grid;
  grid-template-columns: 225px;
  grid-column-gap: 50px;
  grid-row-gap: 45px;
  justify-content: center;
}

@media screen and (min-width: 768px) and (max-width: 1023px) {
  .wrapper_grid {
    display: grid;
    grid-template-columns: 225px 225px;
    grid-column-gap: 50px;
    grid-row-gap: 45px;
    justify-content: center;
  }
}

@media screen and (min-width: 1024px) {
  .wrapper_grid {
    display: grid;
    grid-template-columns: 225px 225px 225px;
    grid-column-gap: 50px;
    grid-row-gap: 45px;
  }
}

.time_text_color_off_watch {
  color: #9E9E9E;
}

.watch {
  height: 120px;
  padding-top: 22px;
  padding-bottom: 20px;
  background-color: #696969;
}

.time {
  font-size: 22px;
}

.line {
  margin-top: 20px;
  margin-bottom: 20px;
}

.line_off_watch {
  border: 1px solid #9E9E9E;
}

.line_on_watch {
  border: 1px solid #FFF;
}

.watch_buttons {
  padding-left: 75px;
  padding-right: 75px;
}

.play_button {
  float: left;
  background-image: url("../../public/img/svg/play.svg");
  background-repeat: no-repeat;
}

.stop_button {
  float: left;
  background-image: url("../../public/img/svg/stop.svg");
  background-repeat: no-repeat;
}

.reset_button {
  float: right;
}

.reset_button_off_watch {
  background-image: url("../../public/img/svg/reset_off_watch.svg");
  background-repeat: no-repeat;
}

.reset_button_on_watch {
  background-image: url("../../public/img/svg/reset_on_watch.svg");
  background-repeat: no-repeat;
}

.add_button {
  width: 225px;
  height: 120px;
  background-image: url("../../public/img/svg/add_button.svg");
  background-repeat: no-repeat;
}
</style>
