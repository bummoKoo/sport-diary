<template>
  <div class="bg-gray-100">
    <AddFitnessPlan v-if="showFitnessPlan" @onCloseAddFitnessPopup="onCloseAddFitnessPopup" @onAddFitnessPlan="onAddFitnessPlan"></AddFitnessPlan>
    <div>
      <div class="mx-auto h-auto p-4 pb-0 flex items-end justify-end">
        <Button
          title="오늘의 운동 시작하기"
          @onClickButton="onClickStartTraining"
        ></Button>
        <Button
          title="운동 계획 등록하기"
          @onClickButton="onClickNewPlanning"
        ></Button>
      </div>
      <div class="container mx-auto px-4 py-2 md:py-4">
        <div class="bg-white rounded-lg shadow overflow-hidden">
          <div class="flex items-center justify-between py-2 px-6">
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
            <div class="border rounded-lg px-1" style="padding-top: 2px">
              <button
                type="button"
                class="
                  leading-none
                  rounded-lg
                  transition
                  ease-in-out
                  duration-100
                  inline-flex
                  cursor-pointer
                  hover:bg-gray-200
                  p-1
                  items-center
                "
                @click="
                  month--;
                  getNoOfDays();
                "
              >
                <svg
                  class="h-6 w-6 text-gray-500 inline-flex leading-none"
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
              <div class="border-r inline-flex h-6"></div>
              <button
                type="button"
                class="
                  leading-none
                  rounded-lg
                  transition
                  ease-in-out
                  duration-100
                  inline-flex
                  items-center
                  cursor-pointer
                  hover:bg-gray-200
                  p-1
                "
                @click="
                  month++;
                  getNoOfDays();
                "
              >
                <svg
                  class="h-6 w-6 text-gray-500 inline-flex leading-none"
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
          <div class="-mx-1 -mb-1">
            <div class="flex flex-wrap border-t border-l">
              <template v-for="(day, index) in DAYS">
                <div
                  style="width: 14.26%"
                  class="px-2 py-2"
                  :key="'day' + index"
                >
                  <div
                    v-text="day"
                    class="
                      text-gray-600 text-sm
                      uppercase
                      tracking-wide
                      font-bold
                      text-center
                    "
                  ></div>
                </div>
              </template>
            </div>
            <div class="flex flex-wrap border-t border-l">
              <template v-for="(blankday, blankDayIndex) in blankdays">
                <div
                  style="width: 14.28%; height: 120px"
                  class="text-center border-r border-b px-4 pt-2"
                  :key="'blackDay' + blankDayIndex"
                ></div>
              </template>
              <template v-for="(date, dateIndex) in no_of_days">
                <div
                  style="width: 14.28%; height: 120px"
                  class="px-4 pt-2 border-r border-b"
                  :key="'noOfDays' + dateIndex"
                >
                  <div
                    @click="showEventModal(date)"
                    v-text="date"
                    class="
                      inline-flex
                      w-6
                      h-6
                      items-center
                      justify-center
                      cursor-pointer
                      text-center
                      leading-none
                      rounded-full
                      transition
                      ease-in-out
                      duration-100
                    "
                    :class="{
                      'bg-blue-500 text-white': isToday(date) == true,
                      'text-gray-700 hover:bg-blue-200': isToday(date) == false,
                    }"
                  ></div>
                  <div style="height: 80px" class="overflow-y-auto mt-1">
                    <template
                      v-for="(event, eventIndex) in events.filter(
                        (e) =>
                          new Date(e.event_date).toDateString() ===
                          new Date(year, month, date).toDateString()
                      )"
                    >
                      <div
                        class="px-2 py-1 rounded-lg mt-1 overflow-hidden border"
                        :class="{
                          'border-blue-200 text-blue-800 bg-blue-100':
                            event.event_theme === 'blue',
                          'border-red-200 text-red-800 bg-red-100':
                            event.event_theme === 'red',
                          'border-yellow-200 text-yellow-800 bg-yellow-100':
                            event.event_theme === 'yellow',
                          'border-green-200 text-green-800 bg-green-100':
                            event.event_theme === 'green',
                          'border-purple-200 text-purple-800 bg-purple-100':
                            event.event_theme === 'purple',
                        }"
                        :key="'event' + eventIndex"
                      >
                        <p
                          v-text="event.event_title"
                          class="text-sm truncate leading-tight"
                        ></p>
                      </div>
                    </template>
                  </div>
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
      month: "",
      year: "",
      no_of_days: [],
      blankdays: [],
      days: ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
      events: [
        {
          event_date: new Date(2021, 9, 1),
          event_title: "April Fool's Day",
          event_theme: "blue",
        },

        {
          event_date: new Date(2021, 9, 10),
          event_title: "Birthday",
          event_theme: "red",
        },

        {
          event_date: new Date(2021, 9, 16),
          event_title: "Upcoming Event",
          event_theme: "green",
        },
      ],
      openEventModal: false,
      showFitnessPlan: false
    };
  },
  methods: {
    isToday(date) {
      const today = new Date();
      const d = new Date(this.year, this.month, date);

      return today.toDateString() === d.toDateString() ? true : false;
    },

    showEventModal(date) {
      // open the modal
      this.openEventModal = true;
      this.event_date = new Date(this.year, this.month, date).toDateString();
    },
    getNoOfDays() {
      if (this.month === 12) {
        this.month = 0;
        this.year++;
      } else if (this.month === -1) {
        this.month = 11;
        this.year--;
      }

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
    onClickStartTraining() {
      alert("오늘 운동 시작하기");
    },
    onClickNewPlanning() {
      this.showFitnessPlan = true;
    },
    onCloseAddFitnessPopup () {
      this.showFitnessPlan = false;
    },
    onAddFitnessPlan () {
      alert("운동 계획이 등록되었습니다.");
    }
  },
  created() {
    this.MONTH_NAMES = [
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
    ];
    this.DAYS = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
  },
  mounted() {
    let today = new Date();
    this.month = today.getMonth();
    this.year = today.getFullYear();
    this.datepickerValue = new Date(
      this.year,
      this.month,
      today.getDate()
    ).toDateString();
    this.getNoOfDays();
  },
};
</script>
<style>
[x-cloak] {
  display: none;
}
</style>