<template>
    <Head>
        <Title>Create Workout</Title>
    </Head>
    <div class="max-w-screen-md mx-auto px-4 py-10">
        <div v-if="statusMessage || errorMessage" class=" mb-10 p-4 bg-[#f1f1f1] shadow-lg rounded-md">
            <p class="text-[#40A578]">{{ statusMessage }}</p>
            <p class="text-red-500">{{ errorMessage }}</p>
        </div>

        <div class="p-8 flex items-start bg-[#f1f1f1] rounded-md shadow-lg">
            <form @submit.prevent="createWorkout" class="flex flex-col gap-y-5 w-full">
                <h1 class=" text-2xl text-[#40A578]">Record Workout</h1>

                <div class="flex flex-col">
                    <label for="workout-name" class=" mb-1 text-sm text-[#40A578]">Workout Name</label>
                    <input type="text" required class=" p-2 text-gray-500 focus:outline-none" id="workout-name"
                        v-model="workoutName">
                </div>
                <div class="flex flex-col">
                    <label for="workout-type" class=" mb-1 text-sm text-[#40A578]">Workout Type</label>
                    <select required class=" p-2 text-gray-500 focus:outline-none" id="workout-type"
                        v-model="workoutType" @change="workoutChange">
                        <option value="select-workout">Select Workout</option>
                        <option value="strength">Strength Training</option>
                        <option value="cardio">Cardio</option>

                    </select>

                </div>

                <!--input for strength exercises-->

                <div v-if="workoutType === 'strength'" class="flex flex-col gap-y-4">
                    <div class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row" v-for="(item, index) in exercises"
                        :Key="index">
                        <div class="flex flex-col md:w-1/3 ">
                            <label for="exercise-name" class="mb-1 text-sm text-[#40A578]">Exercise</label>
                            <input type="text" required class=" p-2 w-full text-gray-500 focus:outline-none "
                                v-model="item.exercise">
                            <!--item.property is created at the addExcercise function, which runs either by the button or when selection is changed.-->
                        </div>
                        <div class="flex flex-col flex-1 ">
                            <label for="sets" class="mb-1 text-sm text-[#40A578]">Sets</label>
                            <input type="text" required class=" p-2 w-full text-gray-500 focus:outline-none "
                                v-model="item.sets">
                            <!--item.property is created at the addExcercise function, which runs either by the button or when selection is changed.-->
                        </div>
                        <div class="flex flex-col flex-1 ">
                            <label for="reps" class="mb-1 text-sm text-[#40A578]">Reps</label>
                            <input type="text" required class=" p-2 w-full text-gray-500 focus:outline-none "
                                v-model="item.reps">
                            <!--item.property is created at the addExcercise function, which runs either by the button or when selection is changed.-->
                        </div>
                        <div class="flex flex-col flex-1 ">
                            <label for="weight" class="mb-1 text-sm text-[#40A578]">Weight (KG)</label>
                            <input type="text" required class=" p-2 w-full text-gray-500 focus:outline-none "
                                v-model="item.weight">
                            <!--item.property is created at the addExcercise function, which runs either by the button or when selection is changed.-->
                        </div>
                        <img src="~/assets/images/trash-light-green.png" @click="deleteExercise(item.id)"
                            class="h-4 w-auto absolute -left-5 cursor-pointer">
                    </div>
                    <button type="button" @click="addExcercise" class="mt-6 py-2 px-6 rounded-sm self-start text-sm text-white bg-[#40A578] duration-200 
            border-solid border-2 border-transparent hover:border-[#40A578] hover:bg-white hover:text-[#40A578]">Add
                        excercie</button>
                </div>

                <!--input for cardio exercises-->

                <div v-if="workoutType === 'cardio'" class="flex flex-col gap-y-4">
                    <div class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row" v-for="(item, index) in exercises"
                        :Key="index">
                        <div class="flex flex-col md:w-1/3 ">
                            <label for="cardio-type" class="mb-1 text-sm text-[#40A578]">Type</label>
                            <select id="cardio-type" class=" p-2 w-full text-gray-500 focus:outline-none"
                                v-model="item.cardioType">
                                <option value="#">Select Type</option>
                                <option value="run">Runs</option>
                                <option value="walk">Walk</option>
                            </select>
                            <!--item.property is created at the addExcercise function, which runs either by the button or when selection is changed.-->
                        </div>

                        <div class="flex flex-col flex-1 ">
                            <label for="distance" class="mb-1 text-sm text-[#40A578]">Distance</label>
                            <input type="text" required class=" p-2 w-full text-gray-500 focus:outline-none "
                                v-model="item.distance">
                        </div>

                        <div class="flex flex-col flex-1 ">
                            <label for="duration" class="mb-1 text-sm text-[#40A578]">Duration</label>
                            <input type="text" required class=" p-2 w-full text-gray-500 focus:outline-none "
                                v-model="item.duration">
                        </div>

                        <div class="flex flex-col flex-1 ">
                            <label for="pace" class="mb-1 text-sm text-[#40A578]">Pace</label>
                            <input type="text" required class=" p-2 w-full text-gray-500 focus:outline-none "
                                v-model="item.pace">
                        </div>

                        <img src="~/assets/images/trash-light-green.png" @click="deleteExercise(item.id)"
                            class="h-4 w-auto absolute -left-5 cursor-pointer">
                    </div>
                    <button type="button" @click="addExcercise" class="mt-6 py-2 px-6 rounded-sm self-start text-sm text-white bg-[#40A578] duration-200 
            border-solid border-2 border-transparent hover:border-[#40A578] hover:bg-white hover:text-[#40A578]">Add
                        excercie</button>
                </div>

                <button type="submit" class="mt-6 py-2 px-6 rounded-sm self-start text-sm text-white bg-[#40A578] duration-200 
            border-solid border-2 border-transparent hover:border-[#40A578] hover:bg-white hover:text-[#40A578]">Record
                    Excercise</button>
            </form>
        </div>

    </div>
</template>

<script setup>
definePageMeta({
    middleware: 'auth'
})

const workoutName = ref('')
const workoutType = ref('select-workout')
const exercises = ref([])
const statusMessage = ref(null)
const errorMessage = ref(null)

const addExcercise = () => { //runs when add excericse is cicked or when workoutType is changed. so an object is pushed so the v-for will run
    if (workoutType.value === 'strength') {
        exercises.value.push({
            id: Math.floor(Math.random() * 1000000),
            exercise: '',
            reps: '',
            sets: '',
            weight: '',
        })
        return //else won't run because of return, if no return, else will run also
    }   //else:
    exercises.value.push({
        id: Math.floor(Math.random() * 1000000),
        cardioType: '',
        distance: '',
        pace: '',
        duration: ''

    })

}

const workoutChange = () => { //runs when workoutType is changed, will run addexercise also
    exercises.value = []
    addExcercise()
}

const deleteExercise = (id) => {
    if (exercises.value.length > 1) {
        exercises.value = exercises.value.filter((item) => item.id !== id)
        return
    } //else:
    errorMessage.value = "Error: cannot remove, need to have at least one exercise"
    setTimeout(() => {
        errorMessage.value = false
    }, 5000);
}


//send to supabase:
const client = useSupabaseClient() 
const user = useSupabaseUser()
console.log(user.value)
const createWorkout = async () => {
    try {
        const { error } = await client.from('workouts').insert({ workoutName: workoutName.value, workoutType: workoutType.value, exercises: exercises.value, userId: user.value.id }) //exercises column is json format in supabase, so use json for array of objects
        if (error) throw error
        statusMessage.value = "Success: Workout created!"
        workoutName.value = ''
        workoutType.value = 'select-workout'
        exercises.value = []
        setTimeout(() => {
            statusMessage.value = false
        }, 5000);
    } catch (error) {
        errorMessage.value = "Error: " + error.errorMessage
        setTimeout(() => {
            errorMessage.value = false
        }, 50000);
    }
}
</script>

<!--whenever we change the selection or click on the add workout button, a new object containing the specific properties, 
    depeneding on the selction
whether it is cardio or strength, is added to the excercises array. Then in the v-for loop the workout type is checked then
 the the inputs are added that many times as the number
of the objects in that array.

when we change the workout type in the selection, workoutChange is run, the excercises array is cleared, so only one type of excercie can be added at at time.
and also when we change the selecion, the addExcercise function is run, so at least an object is pushed in the array, that 
is why the v-for works.-->


<!--some notes for supabase

he used insert template for update and delete also.
you have to write auth.role() = 'authenticated' to allow only authenticated users.

update templaet uses email, so user rows will be specific for them.

use json data type for supabse columns when the data they recieve is an array of objects.-->