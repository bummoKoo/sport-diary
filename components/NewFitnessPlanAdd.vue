<template>
  <b-modal
    header-bg-variant="dark"
    header-text-variant="light"
    id="new-fitness-plan-add"
    size="xl"
    title="운동 계획 등록하기"
    centered
  >
    <b-container>
      <div class="text-lg mb-2">운동 계획 일자</div>
      <div class="flex flex-row mb-3">
        <b-form-input
          class="w-25"
          id="plan-from-date"
          v-model="fromDate"
          type="date"
          placeholder="YYYY-MM-DD"
        ></b-form-input>
        <div>~</div>
        <b-form-input
          class="w-25"
          id="plan-to-date"
          v-model="toDate"
          type="date"
          placeholder="YYYY-MM-DD"
        ></b-form-input>
      </div>
      <div class="text-lg mb-2">운동 계획 시간</div>
      <div class="flex flex-row mb-3">
        <b-form-input
          class="w-25"
          id="plan-from-time"
          v-model="fromTime"
          type="time"
          placeholder="YYYY-MM-DD"
        ></b-form-input>
        <div>~</div>
        <b-form-input
          class="w-25"
          id="plan-to-time"
          v-model="toTime"
          type="time"
          placeholder="YYYY-MM-DD"
        ></b-form-input>
      </div>
      <div class="text-lg mb-2">운동 종목</div>
      <b-row class="mb-1 text-center">
        <b-col>운동 종목</b-col>
        <b-col>세트 수</b-col>
        <b-col>세트 별 쉬는 시간</b-col>
      </b-row>
      <template v-for="(event, index) in addFitnessEventList">
        <b-row class="mb-1 text-center" :key="'add-to-event' + index">
          <b-col>
            <b-form-select v-model="event.name" :options="fitness.nameList"></b-form-select>
          </b-col>
          <b-col>
            <b-form-select v-model="event.sets" :options="fitness.sets"></b-form-select>
          </b-col>
          <b-col>
            <b-form-select v-model="event.restTimes" :options="fitness.restTimes"></b-form-select>
          </b-col>
        </b-row>
      </template>
    </b-container>
  </b-modal>
</template>

<script>
export default {
  data() {
    return {
      fromDate: "",
      toDate: "",
      fromTime: "",
      toTime: "",
      addFitnessEventList: [
        {
          name: "벤치 프레스",
          sets: "3 세트",
          restTimes: "1분 30초"
        },
        {
          name: "레그 프레스",
          sets: "3 세트",
          restTimes: "1분 30초"
        }
      ],
      fitness: {
        nameList: [
          { value: 'benchPress', text: '벤치 프레스' },
          { value: 'incline-benchPress', text: '인클라인 벤치 프레스' },
          { value: 'decline-benchPress', text: '디클라인 벤치 프레스' },
          { value: 'dumbell-benchPress', text: '덤벨 프레스' },
          { value: 'chestPress', text: '체스트 프레스' },
        ],
        sets: [
          { value: '1', text: '1 세트' },
          { value: '2', text: '2 세트' },
          { value: '3', text: '3 세트' },
          { value: '4', text: '4 세트' },
          { value: '5', text: '5 세트' },
        ],
        restTimes: [
          { value: '30', text: '30초' },
          { value: '60', text: '1분' },
          { value: '90', text: '1분 30초' },
          { value: '120', text: '2분' },
          { value: '150', text: '2분 30초' },
        ]
      }
    };
  },
};
</script>