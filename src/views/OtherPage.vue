<script setup>
import { faker } from '@faker-js/faker';
import { ref, watch } from 'vue';

faker.locale = 'en';

const currentDate = ref(new Date());

function getWeekDates(date) {
  const dayOfWeek = date.getDay();
  const startDate = new Date(date);
  startDate.setDate(date.getDate() - dayOfWeek);

  const weekDates = [];
  for (let i = 0; i < 7; i++) {
    const currentDate = new Date(startDate);
    currentDate.setDate(startDate.getDate() + i);
    weekDates.push(currentDate);
  }
  return weekDates;
}

const weekDates = ref(getWeekDates(currentDate.value));
const weekDays = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];

function nextWeek() {
  currentDate.value.setDate(currentDate.value.getDate() + 7);
  weekDates.value = getWeekDates(currentDate.value);
}

function prevWeek() {
  currentDate.value.setDate(currentDate.value.getDate() - 7);
  weekDates.value = getWeekDates(currentDate.value);
}

function generateNotes() {
  return weekDates.value.map(date => {
    const numNotes = Math.floor(Math.random() * 3);
    const notes = [];
    for (let i = 0; i < numNotes; i++) {
      notes.push(faker.lorem.sentence({ min: 3, max: 7 }));
    }
    return { date, notes, avatar: faker.image.avatar() }; 
  });
}

const weekNotes = ref(generateNotes());

function updateNotes() {
  weekNotes.value = generateNotes();
}

watch(weekDates, () => {
  updateNotes();
});
</script>

<template>
  <main class="p-4">
    <div class="flex justify-between items-center mb-4">
      <button @click="prevWeek">Previous Week</button>
      <h2 class="text-2xl font-semibold">
        {{ weekDates[0].toLocaleDateString('en-US', { month: 'long', day: 'numeric' }) }} -
        {{ weekDates[6].toLocaleDateString('en-US', { month: 'long', day: 'numeric' }) }}
      </h2>
      <button @click="nextWeek">Next Week</button>
    </div>
    <div class="grid grid-cols-7 gap-2">
      <div v-for="(day, index) in weekNotes" :key="index" class="border p-2 relative">
        <img :src="day.avatar" class="absolute top-1 right-1 h-8 w-8 rounded-full object-cover" />
        <p class="font-semibold">{{ weekDays[index] }} {{ day.date.getDate() }}</p>
        <ul v-if="day.notes.length">
          <li v-for="(note, noteIndex) in day.notes" :key="noteIndex" class="text-sm">
            {{ note }}
          </li>
        </ul>
      </div>
    </div>
  </main>
</template>