<template>
  <div class="datepicker" @click.stop>
    <slot
      :show="show"
      :formatedValue="formatedValue"
      :class="this.classValue"
      :placeholder="this.placeholder"
    >
      <input
        type="text"
        :class="this.classValue"
        v-model="formatedValue"
        @focus="show()"
        :placeholder="this.placeholder"
      />
    </slot>
    <div
      v-if="visible"
      :width="`${width}px`"
      :class="['calendar', { show: visible }]"
    >
      <div class="calendar__header">
        <div class="calendar__year">{{ formatedYear }}</div>
        <div class="calendar__date">{{ formatedDate }}</div>
      </div>
      <div class="calendar__body">
        <!-- month -->
        <div class="calendar__month">
          <button type="button" class="calendar__month__prev" @click="prev()">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              role="img"
            >
              <path
                d="M8.59,16.58L13.17,12L8.59,7.41L10,6L16,12L10,18L8.59,16.58Z"
              ></path>
            </svg>
          </button>
          <span>{{ formatedYearOrMonth }}</span>
          <select
            @change="monthSelectChange"
            v-model="monthValue"
            size="mini"
            v-if="this.monthSelect"
            style=""
          >
            <option
              style="text-align-last: center"
              v-for="(month, index) in getMonthsList"
              :key="month"
              :label="month"
              :value="index"
            >
              <!-- {{ month }} -->
            </option>
          </select>
          <select
            @change="yearSelectChange"
            v-model="yearValue"
            size="mini"
            v-if="this.yearSelect"
            style="margin-left: 5px"
          >
            <option
              style="text-align-last: center"
              v-for="i in numberofYears"
              :key="i"
              :value="startingYear + (i - 1)"
              :label="
                formatNepali
                  ? getNepaliDateWithYear(startingYear + (i - 1)).substr(0, 4)
                  : startingYear + (i - 1)
              "
            ></option>
          </select>
          <button type="button" icon="el-icon-arrow-right" @click="next()">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              role="img"
            >
              <path
                d="M8.59,16.58L13.17,12L8.59,7.41L10,6L16,12L10,18L8.59,16.58Z"
              ></path>
            </svg>
          </button>
        </div>
        <!-- week days -->
        <div
          style="padding: 3px"
          :style="showToday ? '' : 'padding-bottom: 10px;'"
        >
          <div class="calendar__weeks">
            <div
              class="calendar__weekday"
              v-for="(weekday, w) in weekdays"
              :key="w"
            >
              {{ weekday }}
            </div>
          </div>
          <!-- days of month -->
          <div class="calendar__days">
            <div
              class="calendar__day_spacer"
              :style="{ gridColumn: `span ${startweek}` }"
            ></div>
            <div
              :class="[
                'calendar__day',
                { selected: active(day) },
                { today: checkToday(day) },
              ]"
              v-for="(day, d) in days"
              :key="d"
              @click="select(day)"
            >
              {{ formatNepali ? convertToNepali(day).substr(8, 10) : day.day }}
            </div>
          </div>
        </div>
      </div>
      <div v-if="showToday" class="calendar__footer">
        <button type="button" @click="today()">{{ formatedTodayText }}</button>
      </div>
    </div>
  </div>
</template>

<script>
import NepaliDate from "nepali-date/cjs/NepaliDate";
import {
  YEAR_DATES,
  ENGLISH_WEEK,
  NEPALI_WEEK,
  NEPALI_MONTH,
  ENGLISH_NEPALI_MONTH,
} from "./constants.js";
export default {
  name: "VNepalidatepicker", // vue component name
  props: {
    value: { type: String, default: "" },
    format: { type: String, default: "YYYY-MM-DD" },
    calendarType: { type: String, default: "English" },
    yearSelect: { type: Boolean, default: true },
    monthSelect: { type: Boolean, default: true },
    classValue: { type: String, default: "" },
    placeholder: { type: String, default: "" },
    showToday: { type: Boolean, default: false },
    width: { type: Number, default: 250 },
  },
  model: {
    event: "change",
  },
  data() {
    return {
      date: this.value == "" ? new NepaliDate() : new NepaliDate(this.value),
      formatedValue: this.value,
      visible: false,
      startingYear: 2001,
      numberofYears: 87,
      formatNepali: this.calendarType == "Nepali" ? true : false,
      endDay: null,
      yearValue:
        this.value == ""
          ? new NepaliDate().getYear()
          : new NepaliDate(this.value).getYear(),
      monthValue:
        this.value == ""
          ? new NepaliDate().getMonth()
          : new NepaliDate(this.value).getMonth(),
      startMonthValue: null,
      currentDateValue: undefined,
    };
  },
  computed: {
    getMonthsList() {
      return this.formatNepali ? NEPALI_MONTH : ENGLISH_NEPALI_MONTH;
    },
    year() {
      return this.date.year;
    },
    weekdays() {
      return this.formatNepali ? NEPALI_WEEK : ENGLISH_WEEK;
    },
    days() {
      YEAR_DATES.forEach(yearData => {
        if (yearData.year == this.date.year) {
          yearData.value.forEach((data, index) => {
            // compare monthValue selected to index of yearData value
            if (index == this.date.month) {
              this.endDay = data; // eslint-disable-line
            }
          });
        }
      });
      return Array(this.endDay)
        .fill()
        .map((_, idx) => new NepaliDate(this.year, this.date.month, idx + 1));
    },
    startweek() {
      let currentDateValue = new NepaliDate(this.yearValue, this.monthValue, 1);

      ENGLISH_WEEK.forEach((data, index) => {
        if (currentDateValue.format("DDD") == "Sun") {
          this.startMonthValue = 7; // eslint-disable-line
        } else if (currentDateValue.format("DDD") == data) {
          this.startMonthValue = index; // eslint-disable-line
        }
      });
      return this.startMonthValue;
    },
    formatedYearOrMonth() {
      if (this.monthSelect == false && this.yearSelect == false) {
        return this.formatNepali
          ? this.date.format("mmmm yyyy")
          : this.date.format("MMMM YYYY");
      }

      if (this.monthSelect == false) {
        return this.formatNepali
          ? this.date.format("mmmm")
          : this.date.format("MMMM");
      }

      if (this.yearSelect == false) {
        return this.formatNepali
          ? this.date.format("yyyy")
          : this.date.format("YYYY");
      }
      return "";
    },
    formatedYear() {
      return this.formatNepali
        ? this.date.format("yyyy")
        : this.date.format("YYYY");
    },
    formatedDate() {
      return this.formatNepali
        ? this.date.format("dddd, dd mmmm")
        : this.date.format("DDDD, DD MMMM");
    },
    formatedTodayText() {
      return this.formatNepali ? "आज" : "Today";
    },
  },
  methods: {
    convertToNepali(date) {
      return new NepaliDate(date).format("yyyy-mm-d");
    },
    getNepaliDateWithYear(year) {
      return new NepaliDate(year, 0, 1).format("yyyy-mm-d");
    },
    active(date) {
      return this.date.getTime() === date.getTime();
    },
    checkToday(date) {
      let today = new NepaliDate();
      return (
        date.day == today.day &&
        date.year == today.year &&
        date.month == today.month
      );
    },
    next() {
      let _month = this.date.month + 1;
      let _year = this.date.year;
      if (_month > 11) {
        _year++;
        _month = 0;
      }
      this.setMonthAndYear(_month, _year);
      this.date = new NepaliDate(_year, _month, 1);
    },
    prev() {
      let _month = this.date.month - 1;
      let _year = this.date.year;
      if (_month < 0) {
        _year--;
        _month = 11;
      }
      this.setMonthAndYear(_month, _year);
      this.date = new NepaliDate(_year, _month, 1);
    },
    monthSelectChange() {
      this.date.setMonth(this.monthValue);
    },
    yearSelectChange() {
      this.date.setYear(this.yearValue);
    },
    select(date) {
      this.date = date;
      this.formatedValue = this.date.format(this.format);

      let nepaliDate = new NepaliDate(this.formatedValue);
      let engDate = new Date(
        nepaliDate.getEnglishDate() - new Date().getTimezoneOffset() * 60000
      )
        .toISOString()
        .substr(0, 10);

      this.$emit("ad", engDate);
      this.$emit("change", this.formatedValue);
      this.$emit("input", this.formatedValue);
      this.hide();
    },
    show() {
      this.visible = true;
      setTimeout(() => document.addEventListener("click", this.hide), 200);
    },
    hide() {
      this.visible = false;
      document.removeEventListener("click", this.hide);
    },
    today() {
      this.formatedValue = new NepaliDate().format(this.format);
      this.date = new NepaliDate();
      this.setMonthAndYear(this.date.getMonth(), this.date.getYear());
      this.hide();
    },
    setMonthAndYear(month, year) {
      this.monthValue = month;
      this.yearValue = year;
    },
  },
};
</script>

<style>
:root {
  --primary-color-base: #5495c5;
  --primary-color-dark: #247ac4;
  --secondary-color-base: #dfeffc;
  --primary-radius: 5px;
  --calendar-day-selected-bg-color: #248ac4;
  --calendar-day-today-bg-color: #f77777;
  --calendar-day-border-radius: 3px;
  --calendar-day-bg-color: white;
  --calendar-day-text-color: #1c94b7;
  --calendar-day-font-weight: normal;
  --calendar-week-font-weight: normal;
  --calendar-select-border-radius: 5px;
}
</style>

<style scoped>
* {
  margin: 0;
  box-sizing: border-box;
  font-family: "Open Sans", sans-serif;
}
.datepicker {
  position: relative;
}
.datepicker button {
  outline: none;
  border: 0;
  background: transparent;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
}
.calendar {
  z-index: 9;
  position: absolute;
  top: 100%;
  box-shadow: 0px 14px 45px rgba(0, 0, 0, 0.25),
    0px 10px 18px rgba(0, 0, 0, 0.22);
  background: #fff;
  visibility: hidden;
  opacity: 0;
  border-radius: var(--primary-radius);
  overflow: hidden;
  /* transform: translateY(-50%) translateX(50%); */
  /* transition: all 0.3s linear; */
}
.calendar.show {
  visibility: visible;
  opacity: 1;
  /* transform: translateY(0px); */
}
.calendar__header {
  padding: 15px 10px;
  /* border-radius: 10px; */
  background: var(--primary-color-base);
  color: #fff;
}
.calendar__body {
  padding-top: 7px;
  padding-left: 10px;
  padding-right: 10px;
}
.calendar__year {
  opacity: 0.6;
  font-size: 1rem;
  line-height: 1.2rem;
}
.calendar__date {
  font-size: 1.2rem;
  line-height: 1.5rem;
}
.calendar__month {
  padding: 5px 3px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.calendar__month select {
  height: 28px;
  width: 100px;
  border: none;
  border-radius: var(--calendar-day-border-radius);
  text-align-last: center;
}
.calendar__month button {
  width: 36px;
  height: 36px;
  padding: 7px;
  margin-right: 4px;
  margin-left: 4px;
  border-radius: 50%;
  color: white;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
}
.calendar__month button:hover {
  background: #dedede;
}
.calendar__month__prev {
  transform: rotate(180deg);
}
.calendar__weeks,
.calendar__days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}
.calendar__days {
  gap: 4px;
  padding-top: 10px;
}
.calendar__day,
.calendar__weekday {
  text-align: center;
  font-size: 12px;
}
.calendar__weekday {
  opacity: 0.8;
  font-weight: var(--calendar-week-font-weight);
}
.calendar__day {
  width: 32px;
  height: 32px;
  line-height: 32px;
  font-weight: var(--calendar-day-font-weight);
  cursor: pointer;
  border-radius: var(--calendar-day-border-radius);
  background: var(--calendar-day-bg-color);
  color: var(--calendar-day-text-color);
  /* transition: all 0.3s ease-in-out; */
}
.calendar__day.selected {
  background: var(--calendar-day-selected-bg-color);
  color: #fff;
}
.calendar__day.today {
  background: var(--calendar-day-today-bg-color);
  color: #fff;
}
.calendar__day:hover {
  background: var(--primary-color-base);
  color: #fff;
  opacity: 0.8;
}
.calendar__footer {
  text-align: right;
}
.calendar__footer button {
  padding: 8px 10px;
  text-transform: uppercase;
  font-weight: bold;
  color: rebeccapurple;
  opacity: 0.9;
}
.calendar__footer button:hover {
  opacity: 1;
}
</style>
