<template>
    <div 
        class="w-2/5 mx-auto py-6 px-10 my-14 rounded-lg box-shadow bg-neutral-100 border-2 border-solid border-orange-500"
    >
        <h1 class="text-3xl text-center font-semibold">AGE CALCULATOR</h1>
        <div class="grid grid-cols-3 gap-x-3 mt-8">
            <label class="block">
                <span class="block text-base font-medium text-slate-700">Date</span>
                <input 
                    type="text" 
                    class="mt-1 block w-full px-3 py-2 bg-white border border-slate-300 rounded-md text-base shadow-sm placeholder-slate-400 focus:outline-none focus:border-orange-500 focus:ring-1 focus:ring-orange-500 invalid:border-pink-500 invalid:text-pink-600 focus:invalid:border-pink-500 focus:invalid:ring-pink-500"
                    placeholder="DD"
                    v-model="day"
                    @input="validateDay"
                    maxlength="2"
                />
            </label>
            <label class="block">
                <span class="block text-base font-medium text-slate-700">Month</span>
                <input 
                    type="text"
                    class="mt-1 block w-full px-3 py-2 bg-white border border-slate-300 rounded-md text-base shadow-sm placeholder-slate-400 focus:outline-none focus:border-orange-500 focus:ring-1 focus:ring-orange-500 invalid:border-pink-500 invalid:text-pink-600 focus:invalid:border-pink-500 focus:invalid:ring-pink-500"
                    placeholder="MM"
                    v-model="month"
                    @input="validateMonth"
                    maxlength="2"
                />
            </label>
            <label class="block">
                <span class="block text-base font-medium text-slate-700">Year</span>
                <input 
                    type="text"
                    class="mt-1 block w-full px-3 py-2 bg-white border border-slate-300 rounded-md text-base shadow-sm placeholder-slate-400 focus:outline-none focus:border-orange-500 focus:ring-1 focus:ring-orange-500 invalid:border-pink-500 invalid:text-pink-600 focus:invalid:border-pink-500 focus:invalid:ring-pink-500"
                    placeholder="YYYY"
                    v-model="year"
                    @input="validateYear"
                    maxlength="4"
                />
            </label>
        </div>
        <div class="w-full flex justify-center flex-col mt-6">
            <button 
                class="mx-auto border-2 w-1/5 border-solid border-orange-500 hover:bg-orange-500 hover:text-white bg-white rounded-md px-4 py-1"
                @click="handleClick"
            >
                {{ showResult ? 'Reset' : 'Check' }}
            </button>
        </div>

        <div :class="`${showResult ? 'block':'hidden'} mt-16 text-lg`">
            <h1 class="text-4xl font-semibold text-center">Result</h1>
            <div class="grid grid-cols-1 gap-y-2 mx-auto my-6 bg-white py-5 rounded-lg">
                <div class="flex flex-col text-center">
                    <span>Born On</span>
                    <span class="font-bold text-orange-500">{{ born !== '-' ? born.format('dddd, MMMM D, YYYY') : '-' }}</span>
                </div>
                <div class="flex flex-col text-center">
                    <span>Date Today</span>
                    <span class="font-bold text-orange-500">{{ now.format('dddd, MMMM D, YYYY') }}</span>
                </div>
            </div>
            <div class="grid grid-cols-2 bg-white py-5 rounded-lg">
                <div>
                    <h1 class="text-5xl text-center">Age</h1>
                    <div class="text-center w-9/12 mx-auto mt-4">
                        <div>
                            <span class="me-2 text-6xl text-orange-500">{{ ages.resultYear }}</span>
                            <span>Years</span>
                        </div>
                        <div class="flex justify-center items-center mt-2">
                            <div>
                                <span class="me-2 text-xl">{{ ages.resultMonth }}</span>
                                <span>Months</span>
                            </div>
                            <div class="w-[1px] bg-black mx-4 min-h-5"></div>
                            <div>
                                <span class="me-2 text-xl">{{ ages.resultDay }}</span>
                                <span>Days</span>
                            </div>
                        </div>
                    </div>
                </div>
                <div>
                    <h1 class="text-xl text-center text-orange-500 font-medium">Next Birthday</h1>
                    <div class="flex justify-center py-3 mt-2 bg-orange-500 w-[55px] mx-auto rounded-full">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512" width="30" height="30" fill="white">
                            <path d="M86.4 5.5L61.8 47.6C58 54.1 56 61.6 56 69.2L56 72c0 22.1 17.9 40 40 40s40-17.9 40-40l0-2.8c0-7.6-2-15-5.8-21.6L105.6 5.5C103.6 2.1 100 0 96 0s-7.6 2.1-9.6 5.5zm128 0L189.8 47.6c-3.8 6.5-5.8 14-5.8 21.6l0 2.8c0 22.1 17.9 40 40 40s40-17.9 40-40l0-2.8c0-7.6-2-15-5.8-21.6L233.6 5.5C231.6 2.1 228 0 224 0s-7.6 2.1-9.6 5.5zM317.8 47.6c-3.8 6.5-5.8 14-5.8 21.6l0 2.8c0 22.1 17.9 40 40 40s40-17.9 40-40l0-2.8c0-7.6-2-15-5.8-21.6L361.6 5.5C359.6 2.1 356 0 352 0s-7.6 2.1-9.6 5.5L317.8 47.6zM128 176c0-17.7-14.3-32-32-32s-32 14.3-32 32l0 48c-35.3 0-64 28.7-64 64l0 71c8.3 5.2 18.1 9 28.8 9c13.5 0 27.2-6.1 38.4-13.4c5.4-3.5 9.9-7.1 13-9.7c1.5-1.3 2.7-2.4 3.5-3.1c.4-.4 .7-.6 .8-.8l.1-.1s0 0 0 0s0 0 0 0s0 0 0 0s0 0 0 0c3.1-3.2 7.4-4.9 11.9-4.8s8.6 2.1 11.6 5.4c0 0 0 0 0 0s0 0 0 0l.1 .1c.1 .1 .4 .4 .7 .7c.7 .7 1.7 1.7 3.1 3c2.8 2.6 6.8 6.1 11.8 9.5c10.2 7.1 23 13.1 36.3 13.1s26.1-6 36.3-13.1c5-3.5 9-6.9 11.8-9.5c1.4-1.3 2.4-2.3 3.1-3c.3-.3 .6-.6 .7-.7l.1-.1c3-3.5 7.4-5.4 12-5.4s9 2 12 5.4l.1 .1c.1 .1 .4 .4 .7 .7c.7 .7 1.7 1.7 3.1 3c2.8 2.6 6.8 6.1 11.8 9.5c10.2 7.1 23 13.1 36.3 13.1s26.1-6 36.3-13.1c5-3.5 9-6.9 11.8-9.5c1.4-1.3 2.4-2.3 3.1-3c.3-.3 .6-.6 .7-.7l.1-.1c2.9-3.4 7.1-5.3 11.6-5.4s8.7 1.6 11.9 4.8c0 0 0 0 0 0s0 0 0 0s0 0 0 0l.1 .1c.2 .2 .4 .4 .8 .8c.8 .7 1.9 1.8 3.5 3.1c3.1 2.6 7.5 6.2 13 9.7c11.2 7.3 24.9 13.4 38.4 13.4c10.7 0 20.5-3.9 28.8-9l0-71c0-35.3-28.7-64-64-64l0-48c0-17.7-14.3-32-32-32s-32 14.3-32 32l0 48-64 0 0-48c0-17.7-14.3-32-32-32s-32 14.3-32 32l0 48-64 0 0-48zM448 394.6c-8.5 3.3-18.2 5.4-28.8 5.4c-22.5 0-42.4-9.9-55.8-18.6c-4.1-2.7-7.8-5.4-10.9-7.8c-2.8 2.4-6.1 5-9.8 7.5C329.8 390 310.6 400 288 400s-41.8-10-54.6-18.9c-3.5-2.4-6.7-4.9-9.4-7.2c-2.7 2.3-5.9 4.7-9.4 7.2C201.8 390 182.6 400 160 400s-41.8-10-54.6-18.9c-3.7-2.6-7-5.2-9.8-7.5c-3.1 2.4-6.8 5.1-10.9 7.8C71.2 390.1 51.3 400 28.8 400c-10.6 0-20.3-2.2-28.8-5.4L0 480c0 17.7 14.3 32 32 32l384 0c17.7 0 32-14.3 32-32l0-85.4z"/>
                        </svg>
                    </div>
                    <h1 class="text-center text-xl mt-2">{{ nextBirthday.dayOfWeek }}</h1>
                    <div class="flex justify-center items-center mt-2">
                        <div>
                            <span class="me-2 text-xl">{{ nextBirthday.resultMonth }}</span>
                            <span>Months</span>
                        </div>
                        <div class="w-[1px] bg-black mx-3 min-h-5"></div>
                        <div>
                            <span class="me-2 text-xl">{{ nextBirthday.resultDay }}</span>
                            <span>Days</span>
                        </div>
                    </div>
                </div>
            </div>
            <div class="mt-5 bg-white py-5 rounded-lg">
                <h1 class="text-2xl text-center text-orange-500 font-medium">You're already alive</h1>
                <div class="grid grid-cols-3 text-center w-9/12 mx-auto mt-3">
                    <div>
                        <p class="text-lg">Years</p>
                        <p class="text-2xl font-bold">{{ age.resultYear }}</p>
                    </div>
                    <div>
                        <p class="text-lg">Months</p>
                        <p class="text-2xl font-bold">{{ age.resultMonth }}</p>
                    </div>
                    <div>
                        <p class="text-lg">Weeks</p>
                        <p class="text-2xl font-bold">{{ age.resultWeek }}</p>
                    </div>
                </div>
                <div class="grid grid-cols-3 text-center w-9/12 mx-auto mt-3">
                    <div>
                        <p class="text-lg">Days</p>
                        <p class="text-2xl font-bold">{{ age.resultDay }}</p>
                    </div>
                    <div>
                        <p class="text-lg">Hours</p>
                        <p class="text-2xl font-bold">{{ age.resultHour }}</p>
                    </div>
                    <div>
                        <p class="text-lg">Minutes</p>
                        <p class="text-2xl font-bold">{{ age.resultMinute }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
    interface Age {
        resultDay: number;
        resultMonth: number;
        resultYear: number;
        resultWeek?: number;
        resultHour?: number;
        resultMinute?: number;
    }

    interface NextBirthday {
        resultDay: number;
        resultMonth: number;
        dayOfWeek: string;
    }

    import { ref } from 'vue'
    import dayjs from 'dayjs'
    
    const now = dayjs()
    const dayNow = dayjs().date()
    const monthNow = dayjs().month() + 1
    const yearNow = dayjs().year()

    const day = ref<string>('')
    const month = ref<string>('')
    const year = ref<string>('')

    const born = ref<string>('-')

    const age = ref<Age>({
        resultDay: 0,
        resultMonth: 0,
        resultYear: 0,
        resultWeek: 0,
        resultHour: 0,
        resultMinute: 0,
    })

    const ages = ref<Age>({
        resultDay: 0,
        resultMonth: 0,
        resultYear: 0
    })

    const nextBirthday = ref<NextBirthday>({
        resultDay: 0,
        resultMonth: 0,
        dayOfWeek: '-'
    })

    const showResult = ref<boolean>(false)

    const listMonth = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]

    const ageMessage = ref<string>('')

    const validateDay = (event: Event) => {
        const input = event.target as HTMLInputElement;
        input.value = input.value.replace(/\D/g, '');

        day.value = input.value;
    }

    const validateMonth = (event: Event) => {
        const input = event.target as HTMLInputElement;
        input.value = input.value.replace(/\D/g, '');

        month.value = input.value;
    }

    const validateYear = (event: Event) => {
        const input = event.target as HTMLInputElement;
        input.value = input.value.replace(/\D/g, '');

        year.value = input.value;
    }

    const calculateCurrentYearAge = () => {
        const currentYearBirthDate = dayjs(`${yearNow}-${month.value}-${day.value}`);
        const currentYearAgeMonths = now.diff(currentYearBirthDate, 'month');
        const updatedCurrentYearBirthDateWithMonths = currentYearBirthDate.add(currentYearAgeMonths, 'month');
        const currentYearAgeDays = now.diff(updatedCurrentYearBirthDateWithMonths, 'day');

        ages.value = {
            resultDay: currentYearAgeDays,
            resultMonth: currentYearAgeMonths,
            resultYear: age.value.resultYear,
        }
    }

    const calculateAge = (birthDate: dayjs.Dayjs) => {
        const ageYears = now.diff(birthDate, 'year');
        const updatedBirthDate = birthDate.add(ageYears, 'year');

        const ageMonths = now.diff(birthDate, 'month');
        const updatedBirthDateWithMonths = updatedBirthDate.add(ageMonths, 'month');

        const ageDays = now.diff(birthDate, 'day');
        const updatedBirthDateWithDays = updatedBirthDateWithMonths.add(ageDays, 'day');

        const ageWeeks = Math.floor(ageDays / 7);

        const ageHours = now.diff(birthDate, 'hour');

        const ageMinutes = now.diff(birthDate, 'minute');

        return {
            resultYear: ageYears,
            resultMonth: ageMonths,
            resultDay: ageDays,
            resultWeek: ageWeeks,
            resultHour: ageHours,
            resultMinute: ageMinutes 
        };
    }

    const calculateNextBirthday = () => {
        const thisYearBirthday = dayjs(`${yearNow}-${month.value}-${day.value}`);
        let nextBirthdayDate = thisYearBirthday;

        if (now.isAfter(thisYearBirthday)) {
            nextBirthdayDate = thisYearBirthday.add(1, 'year');
        }

        const monthsUntilNextBirthday = nextBirthdayDate.diff(now, 'month');
        const daysUntilNextBirthday = nextBirthdayDate.diff(now.add(monthsUntilNextBirthday, 'month'), 'day');
        const dayOfWeek = nextBirthdayDate.format('dddd');

        nextBirthday.value = {
            resultDay: daysUntilNextBirthday,
            resultMonth: monthsUntilNextBirthday,
            dayOfWeek: dayOfWeek
        }
    }

    const checkAge = () => {
        if (!(parseInt(day.value)) || !(parseInt(month.value)) || !(parseInt(year.value))) {
            alert('Please enter a valid date')
            return
        }

        if ((year.value).length < 4) {
            alert('Year must be at least 4 digits!')
            return
        }


        const birthDate = dayjs(`${year.value}-${month.value}-${day.value}`);
        born.value = birthDate

        age.value = calculateAge(birthDate)

        calculateCurrentYearAge()
        calculateNextBirthday()

        showResult.value = true
        
        // ageMessage.value = `Your age is ${ageYears} years ${ageMonths} months ${ageDays} days`;
    }

    const resetAge = () => {
        age.value = {
            resultDay: 0,
            resultMonth: 0,
            resultYear: 0,
            resultWeek: 0,
            resultHour: 0,
            resultMinute: 0,
        }

        ages.value = {
            resultDay: 0,
            resultMonth: 0,
            resultYear: 0
        }

        nextBirthday.value = {
            resultDay: 0,
            resultMonth: 0,
            dayOfWeek: '-'
        }

        day.value = ''
        month.value = ''
        year.value = ''
        born.value = '-'
        showResult.value = false;
    }

    const handleClick = () => {
        if (showResult.value) {
            resetAge();
        } else {
            checkAge();
        }
    }
</script>

<style scoped>
    .box-shadow {
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.6);
    }

    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }
</style>