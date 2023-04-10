<template>
  <main class="main">
    <div class="calendar">
      <div class="calendar-header">
        <div class="calendar-header__month">
          <span @click="prevOrNextMonth('prev')" class="month-change prev">
            <icon-base name="next_icon" />
          </span>
          <span>{{ months[currentMonth] }}</span>
          <span @click="prevOrNextMonth('next')" class="month-change">
            <icon-base name="next_icon" />
          </span>
        </div>

        <!-- date picker -->
        <div class="data-picker">
          <picker-input
            @get-date="getDate"
            v-for="(item, index) in pickerInput"
            :key="index"
            v-bind="{
              label: item.label,
              pickerData: item.pickerData,
            }"
          />
        </div>
      </div>

      <div class="calendar-body">
        <div class="calendar-body__week-day">
          <span v-for="(item, index) in weekDays" :key="index">{{ item }}</span>
        </div>
        <div class="calendar-body__day">
          <span
            v-for="item in days"
            :key="item"
            :class="{
              'current-day': isToday(item.day),
              inactive: item.inactive,
            }"
          >
            {{ item.day }}
          </span>
        </div>
      </div>

      <div class="calendar-footer">
        <button class="calendar-footer__submit">Submit</button>
      </div>
    </div>
  </main>
</template>

<script>
import PickerInput from "@/components/PickerInput.vue";
import IconBase from "@/components/IconBase.vue";
export default {
  components: { PickerInput, IconBase },
  data() {
    return {
      date: new Date(),
      currentDate: new Date().getDate(),
      currentMonth: new Date().getMonth(),
      currentYear: new Date().getFullYear(),
      days: [],
      weekDays: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
      months: [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ],
      pickerInput: [
        {
          label: "Year",
          placeholder: "Select",
          pickerData: [
            {
              title: "2020",
              value: 2020,
            },
            {
              title: "2021",
              value: 2021,
            },
            {
              title: "2022",
              value: 2022,
            },
            {
              title: "2023",
              value: 2023,
            },
            {
              title: "2024",
              value: 2024,
            },
            {
              title: "2025",
              value: 2025,
            },
            {
              title: "2026",
              value: 2026,
            },
          ],
        },
        {
          label: "Month",
          placeholder: "Select",
          pickerData: [
            {
              title: "January",
              value: 0,
              isMonth: true,
            },
            {
              title: "February",
              value: 1,
              isMonth: true,
            },
            {
              title: "March",
              value: 2,
              isMonth: true,
            },
            {
              title: "April",
              value: 3,
              isMonth: true,
            },
            {
              title: "May",
              value: 4,
              isMonth: true,
            },
            {
              title: "Jun",
              value: 5,
              isMonth: true,
            },
            {
              title: "July",
              value: 6,
              isMonth: true,
            },
            {
              title: "August",
              value: 7,
              isMonth: true,
            },
            {
              title: "September",
              value: 8,
              isMonth: true,
            },
            {
              title: "October",
              value: 9,
              isMonth: true,
            },
            {
              title: "November",
              value: 10,
              isMonth: true,
            },
            {
              title: "December",
              value: 11,
              isMonth: true,
            },
          ],
        },
      ],
    };
  },
  computed: {
    isToday() {
      return (day) => {
        return day === this.currentDate &&
          this.currentMonth === new Date().getMonth() &&
          this.currentYear === new Date().getFullYear()
          ? true
          : false;
      };
    },
    firtsDayOfMonth() {
      return new Date(this.currentYear, this.currentMonth, 1).getDay(); // getting first day of month
    },
    lastDayOfMonth() {
      return new Date(
        this.currentYear,
        this.currentMonth,
        this.lastDateOfMonth
      ).getDay(); // getting last day of month
    },
    lastDateOfMonth() {
      return new Date(this.currentYear, this.currentMonth + 1, 0).getDate(); // getting last date of month
    },
    lastDateOfLastMonth() {
      return new Date(this.currentYear, this.currentMonth, 0).getDate(); // getting last date of last month
    },
    currentweekDays() {
      return this.date.getDay();
    },

    // isLeapYear() {
    //   return (year) => {
    //     return (
    //       (year % 4 === 0 && year % 100 !== 0 && year % 400 !== 0) ||
    //       (year % 100 === 0 && year % 400 === 0)
    //     );
    //   };
    // },
    // getFebDays() {
    //   return (year) => {
    //     return this.isLeapYear(year) ? 29 : 28;
    //   };
    // },
  },
  mounted() {
    this.renderCalendarDays();
  },
  methods: {
    prevOrNextMonth(evt) {
      if (evt === "prev") {
        this.currentMonth -= 1;
      } else if (evt === "next") {
        this.currentMonth += 1;
      }

      if (this.currentMonth < 0 || this.currentMonth > 11) {
        this.date = new Date(this.currentYear, this.currentMonth);
        this.currentYear = this.date.getFullYear();
        this.currentMonth = this.date.getMonth();
      } else {
        this.date = new Date();
      }
      this.renderCalendarDays();
    },
    renderCalendarDays() {
      this.days = [];
      // creating last days of previous month
      for (let i = this.firtsDayOfMonth; i > 0; i--) {
        this.days.push({
          inactive: true,
          day: this.lastDateOfLastMonth - i + 1,
        });
      }
      // creating all days of current month
      for (let i = 1; i <= this.lastDateOfMonth; i++) {
        this.days.push({
          inactive: false,
          day: i,
        });
      }
      // creating first days of next month
      for (let i = this.lastDayOfMonth; i < 6; i++) {
        this.days.push({
          inactive: true,
          day: i - this.lastDayOfMonth + 1,
        });
      }
    },
    // filter
    getDate(evt) {
      if (evt.hasOwnProperty("isMonth") && evt.isMonth) {
        this.currentMonth = evt.value;
      } else {
        this.currentYear = evt.value;
      }
      this.renderCalendarDays();
    },
  },
};
</script>

<style lang="scss" scoped>
.main {
  display: grid;
  place-content: center;
  min-height: 100vh;
}
.calendar {
  width: max-content;
  height: max-content;
  background-color: #fdfdfd;
  border-radius: 30px;
  padding: 20px;
  overflow: hidden;
  // calendar-header
  &-header {
    // calendar-header__month
    &__month {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      margin-bottom: 1rem;

      span {
        color: #18181b;
        font-size: 1.3rem;
        font-weight: 600;
      }
      .month-change {
        display: grid;
        place-content: center;
        width: 25px;
        height: 25px;
        border-radius: 50%;
        cursor: pointer;
        &:hover {
          background-color: #f4f4f5;
        }
        .icon {
          width: 12px;
          height: 12px;
        }
        &.prev {
          transform: rotate(180deg);
        }
      }
    }
    .data-picker {
      display: flex;
      align-items: center;
      gap: 1rem;
    }
  }

  // calendar-body
  &-body {
    margin-top: 1.2rem;

    // calendar-body__week-day / calendar-body__day
    &__week-day {
      margin-bottom: 1rem;
    }
    &__week-day,
    &__day {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      gap: 0.5rem;
      span {
        display: grid;
        place-content: center;
        width: 45px;
        height: 45px;
        color: #18181b;
        font-weight: 600;
        padding: 12px;
        border-radius: 6px;
        text-align: center;
        cursor: pointer;

        &:hover {
          background-color: #ccfbf1;
        }
        &.weekend {
          background-color: #fce7f3;
        }
        &.inactive {
          color: #e4e4e7;
          visibility: hidden;
        }
        &.current-day {
          color: #fff !important;
          background-color: #14b8a6 !important;
        }
      }
    }
    // span:nth-child(7n-1),
    &__day > span:nth-child(7n-6) {
      background-color: #fce7f3;
    }
  }

  // calendar-footer
  &-footer {
    display: flex;
    justify-content: flex-end;
    margin-top: 2rem;
    // calendar__submit
    &__submit {
      width: 200px;
      outline: none;
      border: none;
      border-radius: 6px;
      background: #14b8a6;
      color: #fff;
      font-size: 1rem;
      font-weight: 600;
      padding: 12px 18px;
      cursor: pointer;
      transition: background 0.2s ease-in-out;
      &:hover {
        opacity: 0.8;
      }
      &:active {
        transform: scale(0.98);
      }
    }
  }
}
</style>
