<template>
    <div 
        class="w-2/5 mx-auto py-6 px-10 my-14 rounded-lg box-shadow"
    >
        <h1 class="text-3xl text-center font-semibold">AGE CALCULATOR</h1>
        <div class="grid grid-cols-3 gap-x-3 mt-8">
            <label class="block">
                <span class="block text-base font-medium text-slate-700">Date</span>
                <input 
                    type="text" 
                    class="mt-1 block w-full px-3 py-2 bg-white border border-slate-300 rounded-md text-base shadow-sm placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500 invalid:border-pink-500 invalid:text-pink-600 focus:invalid:border-pink-500 focus:invalid:ring-pink-500"
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
                    class="mt-1 block w-full px-3 py-2 bg-white border border-slate-300 rounded-md text-base shadow-sm placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500 invalid:border-pink-500 invalid:text-pink-600 focus:invalid:border-pink-500 focus:invalid:ring-pink-500"
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
                    class="mt-1 block w-full px-3 py-2 bg-white border border-slate-300 rounded-md text-base shadow-sm placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500 invalid:border-pink-500 invalid:text-pink-600 focus:invalid:border-pink-500 focus:invalid:ring-pink-500"
                    placeholder="YYYY"
                    v-model="year"
                    @input="validateYear"
                    maxlength="4"
                />
            </label>
        </div>
        <div class="w-full flex justify-center flex-col mt-6">
            <button 
                class="mx-auto border-2 w-1/5 border-solid border-sky-500 hover:bg-sky-500 hover:text-white rounded-md px-4 py-1"
                @click="handleClick"
            >
                {{ showResult ? 'Reset' : 'Check' }}
            </button>
        </div>

        <div :class="`${showResult ? 'block':'hidden'} mt-12 text-lg`">
            <div>
                <h1 class="text-4xl text-center">Age</h1>
                <div class="text-center w-9/12 mx-auto mt-4">
                    <div>
                        <span class="me-2 text-5xl">{{ ages.resultYear }}</span>
                        <span>Years</span>
                    </div>
                    <div class="flex justify-center items-center mt-2">
                        <div>
                            <span class="me-2 text-3xl">{{ ages.resultMonth }}</span>
                            <span>Months</span>
                        </div>
                        <div class="w-[2px] bg-black mx-4 min-h-7"></div>
                        <div>
                            <span class="me-2 text-3xl">{{ ages.resultDay }}</span>
                            <span>Days</span>
                        </div>
                    </div>
                </div>
            </div>
            <div class="mt-5">
                <h1 class="text-2xl text-center">Summary</h1>
                <div class="grid grid-cols-3 text-center w-9/12 mx-auto mt-3">
                    <div>
                        <p class="text-lg">Years</p>
                        <p class="text-2xl font-bold">{{ ageLives.resultYear }}</p>
                    </div>
                    <div>
                        <p class="text-lg">Months</p>
                        <p class="text-2xl font-bold">{{ ageLives.resultMonth }}</p>
                    </div>
                    <div>
                        <p class="text-lg">Days</p>
                        <p class="text-2xl font-bold">{{ ageLives.resultDay }}</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
    interface AgeLives {
        resultDay: number;
        resultMonth: number;
        resultYear: number;
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

    const ageLives = ref<AgeLives>({
        resultDay: 0,
        resultMonth: 0,
        resultYear: 0
    })

    const ages = ref<AgeLives>({
        resultDay: 0,
        resultMonth: 0,
        resultYear: 0
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
            resultYear: ageLives.value.resultYear
        }
    }

    const calculateAge = (birthDate: dayjs.Dayjs) => {
        const ageYears = now.diff(birthDate, 'year');
        const updatedBirthDate = birthDate.add(ageYears, 'year');

        const ageMonths = now.diff(birthDate, 'month');
        const updatedBirthDateWithMonths = updatedBirthDate.add(ageMonths, 'month');

        const ageDays = now.diff(birthDate, 'day');

        return {
            resultDay: ageDays,
            resultMonth: ageMonths,
            resultYear: ageYears
        };
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

        ageLives.value = calculateAge(birthDate)

        calculateCurrentYearAge()

        showResult.value = true
        
        // ageMessage.value = `Your age is ${ageYears} years ${ageMonths} months ${ageDays} days`;
    }

    const resetAge = () => {
        ageLives.value = {
            resultDay: 0,
            resultMonth: 0,
            resultYear: 0
        }

        ages.value = {
            resultDay: 0,
            resultMonth: 0,
            resultYear: 0
        }

        day.value = ''
        month.value = ''
        year.value = ''
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
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
    }

    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
    }
</style>