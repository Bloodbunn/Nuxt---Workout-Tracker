<template>

    <Head>
        <Title>Active Tracker - Home</Title>
    </Head>
    <div>
        <div class="flex justify-center items-center"><h1 v-if="!user" class="p-4 max-md:p-2 text-4xl max-md:text-xl mt-20 max-md:mt-10 mx-auto  text-green-700 font-semibold shadow-md">Log in to see your workouts, or create
            an account to create your workouts!
        </h1></div>
        
        <p v-if="errorMessage" class="text-4xl max-md:text-xl text-red-500 mt-10 max-md:mt-5 mx-auto w-max">{{ errorMessage }}</p>
        <div v-if="user">
            <div v-if="dataLoaded" class=" max-w-screen-lg mx-auto mt-10 max-md:mt-5 px-4 max-md:px-2">
                <!--if exercises-->
                <div v-if="data.length === 0" class="w-full flex flex-col items-center">
                    <h1 class=" text-2xl">Looks empty here...</h1>
                    <NuxtLink to="/create" class="mt-6 py-2 px-6 max-md:mt-3 max-md:py-1 max-md:px-3 rounded-sm text-sm text-white bg-[#40A578] duration-200 
            border-solid border-2 border-transparent hover:border-[#40A578] hover:bg-white hover:text-[#40A578]">Create
                        Workout</NuxtLink>
                </div>

                <!---if exercises-->
                <div v-else class="w-max mx-auto max-md:w-full grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6 max-md:gap-4">
                    <NuxtLink class="flex flex-col items-center bg-[#f1f1f1] p-8 max-md:p-4 shadow-md cursor-pointer"
                        :to="`/${workout.id}`" v-for="(workout, index) in data" :key="index">
                        <!--use v-for becuase data here could be many columns (workout refers to each column)-->
                        <!--cardio img-->
                        <img v-if="workout.workoutType === 'cardio'" src="~/assets/images/running-light-green.png"
                            class=" h-24 max-md:h-12 w-auto">
                        <!--srength image-->
                        <img v-else src="~/assets/images/dumbbell-light-green.png" class="h-24 max-md:h-12 w-auto">
                        <p class=" mt-6 py-1 px-3 text-xs text-white bg-[#40A578] shadow-md rounded-lg">
                            {{ workout.workoutType }}
                        </p>
                        <h1 class=" mt-8 mb-2 text-center text-xl text-[#40A578] ">{{ workout.workoutName }}</h1>
                    </NuxtLink>
                </div>

            </div>
        </div>

    </div>
</template>

<script setup>

const client = useSupabaseClient()
const user = useSupabaseUser()


const data = ref([])
const dataLoaded = ref(null)
const errorMessage = ref(null)

const getData = async () => {
    if (user.value) {
        try {

            const { data: workouts, error } = await client.from('workouts').select('*').eq('userId', user.value.id)
            if (error) throw error
            data.value = workouts
            dataLoaded.value = true

        } catch (error) {
            errorMessage.value = error.message
            setTimeout(() => {
                errorMessage.value = false
            }, 5000);
        }
    } else {
        return
    }

}
onMounted(() => {
    getData()

})


</script>

<!-- the rows of supabase are like objects and the columns are like properties. so workout in data, the workout refers 
to each row, and  so workout.id access the id column, workout.workoutType access the workoutType column
so workout.exercises.cardioType must access the cardioType property of the exercises column-->