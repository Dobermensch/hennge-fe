<template>
  <div class="datepicker-container">
    <div class="datepicker">
      <div class="calendar-icon-container">
        <Calendar width="50" height="20" class="calendar" />
      </div>
      <v-date-picker
        mode="range"
        v-model="dates"
        :is-expanded="false"
      />
    </div>
    <div class="search-icon-container" @click="SearchClicked">
      <SearchIcon width="50" height="20" class="search-icon" />
    </div>
  </div>
</template>

<script>
import Calendar from "@/assets/svg/icon_calender.svg"
import SearchIcon from "@/assets/svg/icon_search.svg"
import { EventBus } from "@/services/event-bus.js"

export default {
  name: 'datepicker',
  components: {
    Calendar,
    SearchIcon
  },
  data() {
    return {
      dates: { start: new Date(new Date().setHours(0,0,0,0)), end: new Date(new Date().setHours(23,59,59,999)) }
    }
  },
  methods: {
    SearchClicked() {
      EventBus.$emit('datesChanged', this.dates);
    }
  },
  mounted() {
    this.SearchClicked();
  }
}
</script>

<style scoped>
.datepicker-container {
  display: flex;
  flex-direction: row;
}

.datepicker {
  display: flex;
  flex-direction: row;
  width: 290px;
  border: 2px solid #F2F2F2;
  border-radius: 12px;
}

.datepicker /deep/ input {
  border: none;
  width: 200px;
  transform: translateX(-12px);
}

.calendar,
.search-icon {
  align-self: center;
}

.calendar-icon-container {
  display: flex;
}

.search-icon-container {
  display: flex;
  width: 50px;
  transform: translateX(-12px);
  background-color: #F2F2F2;
  border-top-right-radius: 12px;
  border-bottom-right-radius: 12px;
  cursor: pointer;
}
</style>
