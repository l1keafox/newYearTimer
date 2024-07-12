<script setup>
import { ref, computed, onMounted} from "vue";
import CountdownHeader from "@/components/CountdownHeader.vue";
import CountdownSegment from "@/components/CountdownSegment.vue";


// const 
const currentDate = new Date();
const newYear = new Date(currentDate.getFullYear()+1,1,1);
const difference =  ref(newYear - currentDate);
const daysInMilisecs = 24 * 60 * 60 * 1000;
const hoursInMilisecs = 60 * 60 * 1000;
const minutesINMilisecs = 60*1000;

// computed
const days = computed(()=>Math.floor(difference.value / daysInMilisecs));
const leftOverAfterDays = computed( ()=> difference.value - (days.value * daysInMilisecs)) ;

const hours = computed(()=>Math.floor(leftOverAfterDays.value / hoursInMilisecs));
const leftOverAfterHours = computed(()=>  leftOverAfterDays.value - (hours.value * hoursInMilisecs) ) ;

const minutes = computed( () =>  Math.floor( leftOverAfterHours.value / minutesINMilisecs  ) );
const leftOverAfterMinutes = computed( () => leftOverAfterHours.value - ( minutes.value * minutesINMilisecs ) )

const seconds = computed(()=>  Math.floor( leftOverAfterMinutes.value/ 1000 ));
// hooks
onMounted(()=>{
  setInterval(()=>{
  difference.value -= 1000;
},1000)
})

</script>
<template>
  <div class="app-wrapper">
    <div class="countdown-box">
      <CountdownHeader />
      <main class="flex justify-center">
        <CountdownSegment data-test="days" label="days" :number="days" />
        <CountdownSegment data-test="hours" label="hours" :number="hours" />
        <CountdownSegment data-test="minutes" label="minutes" :number="minutes" />
        <CountdownSegment data-test="seconds" label="seconds" :number="seconds" />
      </main>
    </div>
  </div>
</template>

<style scoped>
.app-wrapper {
  @apply flex items-center justify-center w-full h-full p-10;
}
.countdown-box {
  @apply shadow-md relative bg-white p-5 rounded-lg border-gray-100 border-[1px];
}
body {
  @apply bg-gray-100;
}
</style>
