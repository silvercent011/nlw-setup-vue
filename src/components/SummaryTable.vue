<template>
  <div class="w-full flex">
    <div class="grid grid-rows-7 grid-flow-row gap-3">
      <div
        v-for="(day, index) in weekDays"
        :key="index"
        class="text-zinc-400 text-xl h-10 w-10 font-bold flex items-center justify-center"
      >
        {{ day }}
      </div>
    </div>

    <div class="grid grid-rows-7 grid-flow-col gap-3" v-if="summary.length > 0">
      <HabitDay
        v-for="date in summaryDates"
        :key="date.toString()"
        :date="date"
        :amount="dayInSummary(date)?.amount"
        :defaultCompleted="dayInSummary(date)?.completed"
      />
      <HabitDay
        v-if="amountOfDaysToFill > 0"
        v-for="date in amountOfDaysToFill"
        class="opacity-40 cursor-not-allowed"
        :key="date"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import HabitDay from "@/components/HabitDay.vue";
import { api } from "@/lib/axios";
import { generateDatesFromYearBeginning } from "@/utils/generate-dates-from-year-beginning";
import dayjs from "dayjs";
import { onMounted, ref } from "vue";

const weekDays = ["D", "S", "T", "Q", "Q", "S", "S"];

const summaryDates = generateDatesFromYearBeginning();

const minimumSummaryDateSize = 18 * 7;

const amountOfDaysToFill = minimumSummaryDateSize - summaryDates.length;

type Summary = {
  id: string;
  date: string;
  amount: number;
  completed: number;
}[];

const summary = ref<Summary>([]);

onMounted(async () => {
  const data = await api.get("summary").then((response) => response.data);
  summary.value = data;
});

function dayInSummary(date: any) {
  const value = summary.value.find((day) =>
    dayjs(date).isSame(day.date, "day")
  );
  return value;
}
</script>
