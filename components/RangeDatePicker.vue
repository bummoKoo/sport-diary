<template>
  <div class="overflow-hidden flex justify-center text-black">
    <div class="flex justify-center">
      <div class="container mx-auto h-auto">
        <div @keydown.escape="closeDatepicker()">
          <div class="flex items-center border rounded-md h-8">
            <div
              @click="
                endToShow = 'from';
                initDate();
                showDatepicker = true;
              "
              v-text="dateFromValue"
              class="
                focus:outline-none
                border-0
                p-2
                w-40
                rounded-l-md
                border-r border-gray-300
              "
            />
            <div class="inline-block px-2 h-auto"> ~ </div>
            <div
              @click="
                endToShow = 'to';
                initDate();
                showDatepicker = true;
              "
              v-text="dateToValue"
              :class="{ 'font-semibold': endToShow == 'to' }"
              class="
                focus:outline-none
                border-0
                p-2
                w-40
                rounded-r-md
                border-l border-gray-300
              "
            />
          </div>
          <div
            class="bg-white mt-2 rounded-lg shadow p-4 absolute top-auto left-auto"
            style="width: 17rem"
            v-show="showDatepicker"
          >
            <div class="flex justify-between items-center mb-2">
              <div>
                <span
                  v-text="MONTH_NAMES[month]"
                  class="text-lg font-bold text-gray-800"
                ></span>
                <span
                  v-text="year"
                  class="ml-1 text-lg text-gray-600 font-normal"
                ></span>
              </div>
              <div>
                <button
                  type="button"
                  class="
                    transition
                    ease-in-out
                    duration-100
                    inline-flex
                    cursor-pointer
                    hover:bg-gray-200
                    p-1
                    rounded-full
                  "
                  @click="
                    if (month == 0) {
                      year--;
                      month = 11;
                    } else {
                      month--;
                    }
                    getNoOfDays();
                  "
                >
                  <svg
                    class="h-6 w-6 text-gray-500 inline-flex"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M15 19l-7-7 7-7"
                    />
                  </svg>
                </button>
                <button
                  type="button"
                  class="
                    transition
                    ease-in-out
                    duration-100
                    inline-flex
                    cursor-pointer
                    hover:bg-gray-200
                    p-1
                    rounded-full
                  "
                  @click="
                    if (month == 11) {
                      year++;
                      month = 0;
                    } else {
                      month++;
                    }
                    getNoOfDays();
                  "
                >
                  <svg
                    class="h-6 w-6 text-gray-500 inline-flex"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M9 5l7 7-7 7"
                    />
                  </svg>
                </button>
              </div>
            </div>

            <div class="flex flex-wrap mb-3 -mx-1">
              <template v-for="(day, index) in DAYS">
                <div style="width: 14.26%" class="px-1" :key="'DAY' + index">
                  <div
                    v-text="day"
                    class="text-gray-800 font-medium text-center text-xs"
                  ></div>
                </div>
              </template>
            </div>

            <div class="flex flex-wrap -mx-1">
              <template v-for="blankday in blankdays">
                <div
                  style="width: 14.28%"
                  class="text-center border p-1 border-transparent text-sm"
                  :key="'BLANK_DAY' + blankday"
                ></div>
              </template>
              <template v-for="(date, dateIndex) in no_of_days">
                <div style="width: 14.28%" :key="'NO_OF_DAYS' + dateIndex">
                  <div
                    @click="getDateValue(date)"
                    v-text="date"
                    class="
                      p-1
                      cursor-pointer
                      text-center text-sm
                      leading-none
                      hover:bg-blue-200
                      leading-loose
                      transition
                      ease-in-out
                      duration-100
                    "
                    :class="{
                      'font-bold': isToday(date) == true,
                      'bg-blue-800 text-white rounded-l-full':
                        isDateFrom(date) == true,
                      'bg-blue-800 text-white rounded-r-full':
                        isDateTo(date) == true,
                      'bg-blue-200': isInRange(date) == true,
                      'cursor-not-allowed opacity-25': isDisabled(date),
                    }"
                    :disabled="isDisabled(date) ? true : false"
                  ></div>
                </div>
              </template>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      MONTH_NAMES: [
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
      DAYS: ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
      showDatepicker: false,
      dateFromYmd: "",
      dateToYmd: "",
      dateFromValue: "",
      dateToValue: "",
      currentDate: null,
      dateFrom: null,
      dateTo: null,
      endToShow: "",
      month: "",
      year: "",
      no_of_days: [],
      blankdays: [],
      currentMonth: "",
      currentYear: "",
    };
  },
  methods: {
    convertFromYmd(dateYmd) {
      const year = Number(dateYmd.substr(0, 4));
      const month = Number(dateYmd.substr(5, 2)) - 1;
      const date = Number(dateYmd.substr(8, 2));

      return new Date(year, month, date);
    },
    convertToYmd(dateObject) {
      const year = dateObject.getFullYear();
      const month = dateObject.getMonth() + 1;
      const date = dateObject.getDate();

      return (
        year + "-" + ("0" + month).slice(-2) + "-" + ("0" + date).slice(-2)
      );
    },
    initDate() {
      if (!this.dateFrom) {
        if (this.dateFromYmd) {
          this.dateFrom = this.convertFromYmd(this.dateFromYmd);
          //} else if ( this.endToShow ) {
          //	this.dateFrom = new Date();
        }
      }
      if (!this.dateTo) {
        if (this.dateToYmd) {
          this.dateTo = this.convertFromYmd(this.dateToYmd);
          //} else if ( this.endToShow ) {
          //	this.dateTo = new Date();
        }
      }
      if (!this.dateFrom) {
        this.dateFrom = this.dateTo;
      }
      if (!this.dateTo) {
        this.dateTo = this.dateFrom;
      }
      if (this.endToShow === "from" && this.dateFrom) {
        this.currentDate = this.dateFrom;
      } else if (this.endToShow === "to" && this.dateTo) {
        this.currentDate = this.dateTo;
      } else {
        this.currentDate = new Date();
      }
      this.currentMonth = this.currentDate.getMonth();
      this.currentYear = this.currentDate.getFullYear();
      if (this.month !== this.currentMonth || this.year !== this.currentYear) {
        this.month = this.currentMonth;
        this.year = this.currentYear;
        this.getNoOfDays();
      }
      this.setDateValues();
    },

    isToday(date) {
      const today = new Date();
      const d = new Date(this.year, this.month, date);

      return today.toDateString() === d.toDateString();
    },

    isDateFrom(date) {
      const d = new Date(this.year, this.month, date);

      return d.toDateString() === this.dateFromValue;
    },

    isDateTo(date) {
      const d = new Date(this.year, this.month, date);

      return d.toDateString() === this.dateToValue;
    },

    isInRange(date) {
      const d = new Date(this.year, this.month, date);

      return d > this.dateFrom && d < this.dateTo;
    },

    isDisabled(date) {
      const d = new Date(this.year, this.month, date);

      if (this.endToShow === "from" && this.dateTo && d > this.dateTo) {
        return true;
      }
      if (this.endToShow === "to" && this.dateFrom && d < this.dateFrom) {
        return true;
      }

      return false;
    },

    setDateValues() {
      if (this.dateFrom) {
        this.dateFromValue = this.dateFrom.toDateString();
        this.dateFromYmd = this.convertToYmd(this.dateFrom);
      }
      if (this.dateTo) {
        this.dateToValue = this.dateTo.toDateString();
        this.dateToYmd = this.convertToYmd(this.dateTo);
      }
    },

    getDateValue(date) {
      let selectedDate = new Date(this.year, this.month, date);
      if (
        this.endToShow === "from" &&
        (!this.dateTo || selectedDate <= this.dateTo)
      ) {
        this.dateFrom = selectedDate;
        if (!this.dateTo) {
          this.dateTo = selectedDate;
        }
      } else if (
        this.endToShow === "to" &&
        (!this.dateFrom || selectedDate >= this.dateFrom)
      ) {
        this.dateTo = selectedDate;
        if (!this.dateFrom) {
          this.dateFrom = selectedDate;
        }
      }
      this.setDateValues();

      this.closeDatepicker();
    },

    getNoOfDays() {
      let daysInMonth = new Date(this.year, this.month + 1, 0).getDate();

      // find where to start calendar day of week
      let dayOfWeek = new Date(this.year, this.month).getDay();
      let blankdaysArray = [];
      for (var i = 1; i <= dayOfWeek; i++) {
        blankdaysArray.push(i);
      }

      let daysArray = [];
      for (var i = 1; i <= daysInMonth; i++) {
        daysArray.push(i);
      }

      this.blankdays = blankdaysArray;
      this.no_of_days = daysArray;
    },

    closeDatepicker() {
      this.endToShow = "";
      this.showDatepicker = false;
    },
  },
  mounted() {
    this.initDate();
  },
};
</script>