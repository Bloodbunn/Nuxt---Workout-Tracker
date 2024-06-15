<template>

    <Head>
        <Title>Workout Details</Title>
    </Head>
    <div class=" max-w-screen-sm mx-auto px-4 py-10 max-md:px-2 max-md:py-5">
        <!--App message-->
        <div v-if="errorMessage || statusMessage" class="p-3 mb-10 max-md:p-2 max-md:mb-5 rounded-md shadow-md bg-[#f1f1f1]">
            <p class="text-red-500 font-bold">{{ errorMessage }}</p>
            <p class="text-green-600 font-bold">{{ statusMessage }}</p>
        </div>

        <div v-if="dataLoaded">

            <!--general workout info card-->
            <div class="flex flex-col items-center p-8 max-md:p-4 rounded-md shadow-md bg-[#f1f1f1] relative">
                <div class="flex absolute left-2 top-2 gap-x-2">
                    <div class="h-7 w-7 rounded-full flex justify-items-center items-center cursor-pointer bg-green-600 shadow-lg"
                        @click="editMode">
                        <img class=" h-3.5 w-auto mx-auto" src="~/assets/images/pencil-light.png" alt="">
                    </div>
                    <div @click="deleteWorkout"
                        class="h-7 w-7 rounded-full flex justify-items-center items-center cursor-pointer bg-green-600 shadow-lg">
                        <img class=" h-3.5 w-auto mx-auto" src="~/assets/images/trash-light.png" alt="">
                    </div>
                </div>
                <!--v-for not used because only single row, so unline in index where we used data as array and we used v-for. but we still need to use v-for for the exercises column because it is an array-->
                <img v-if="data.workoutType === 'cardio'" src="~/assets/images/running-light-green.png"
                    class=" h-24 max-md:h-16 w-auto" alt="">
                <img v-else="data.workoutType === 'strength'" src="~/assets/images/dumbbell-light-green.png"
                    class=" h-24 max-md:h-16 w-auto" alt="">

                <span class="mt-6 max-md:mt-3 py-1.5 px-5 text-sm text-white bg-green-700 rounded-lg shadow-md">{{ data.workoutType
                    }}</span>

                <!--edit mode-->
                <div class="w-full mt-6">
                    <input type="text" v-if="edit" class=" w-full p-2 text-gray-500 focus:outline-none"
                        v-model="data.workoutName"> <!--edit mode-->
                    <h1 v-else class=" text-green-700 text-2xl text-center">{{ data.workoutName }}</h1>
                    <!--workout name is same for both strength and cardio so no need for separate divs-->
                </div>
            </div>



            <!--exercises info, the exercises array differs for strength and cardio, so use separate divs for each-->

            <div class=" mt-5 p-8 max-md:p-4 rounded-md flex flex-col items-center bg-[#f1f1f1] shadow-md">
                <!--stength-->
                <div v-if="data.workoutType === 'strength'" class="flex flex-col gap-y-4 w-full">
                    <div class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
                        v-for="(item, index) in data.exercises" :key="index">
                        <div class="flex flex-2 flex-col ">
                            <!--edit mode-->
                            <label for="exercise-name" class="mb-1 text-sm text-green-700">Exercise</label>
                            <input v-if="edit" id="exercise-name" type="text"
                                class=" p-2 w-full text-gray-500 focus:outline-none" v-model="item.exercise">
                            <!--no edit-->
                            <p v-else>{{ item.exercise }}</p>
                        </div>
                        <div class="flex flex-1 flex-col ">
                            <!--edit mode-->
                            <label for="sets" class="mb-1 text-sm text-green-700">Sets</label>
                            <input v-if="edit" id="sets" type="text"
                                class=" p-2 w-full text-gray-500 focus:outline-none" v-model="item.sets">
                            <p v-else>{{ item.sets }}</p>
                        </div>
                        <div class="flex flex-1 flex-col">
                            <!--edit mode-->
                            <label for="reps" class="mb-1 text-sm text-green-700">Reps</label>
                            <input v-if="edit" id="reps" type="text"
                                class=" p-2 w-full text-gray-500 focus:outline-none" v-model="item.reps">
                            <p v-else>{{ item.reps }}</p>
                        </div>
                        <div class="flex flex-1 flex-col">
                            <!--edit mode-->
                            <label for="weight" class="mb-1 text-sm text-green-700">Weight</label>
                            <input v-if="edit" id="weight" type="text"
                                class=" p-2 w-full text-gray-500 focus:outline-none" v-model="item.weight">
                            <p v-else>{{ item.weight }}</p>
                        </div>

                        <img @click="deleteExercise(item.id)" v-if="edit"
                            class="absolute h-4 w-auto -left-5 cursor-pointer"
                            src="~/assets/images/trash-light-green.png" alt="">

                    </div>

                    <button @click="addExcercise" v-if="edit" type="button" class="mt-6 py-2 px-6 max-md:mt-3 max-md:py-1 max-md:px-3 rounded-sm self-start text-sm text-white bg-[#40A578] duration-200 
            border-solid border-2 border-transparent hover:border-[#40A578] hover:bg-white hover:text-[#40A578]">Add
                        excercie</button>
                </div>

                <!--cardio-->
                <div v-else="data.workoutType === 'cardio'" class="flex flex-col gap-y-4 w-full">
                    <div class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
                        v-for="(item, index) in data.exercises" :key="index">

                        <div class="flex flex-1 flex-col ">
                            <!--edit mode-->
                            <label for="cardioType" class="mb-1 text-sm text-green-700">Type</label>
                            <select v-if="edit" id="cardioType" type="text"
                                class=" p-2 w-full text-gray-500 focus:outline-none" v-model="item.cardioType">
                                <option value="#">Select Type</option>
                                <option value="run">Runs</option>
                                <option value="walk">Walk</option>
                            </select>
                            <p v-else>{{ item.cardioType }}</p>
                        </div>

                        <div class="flex flex-1 flex-col">
                            <!--edit mode-->
                            <label for="distance" class="mb-1 text-sm text-green-700">Distance</label>
                            <input v-if="edit" id="distance" type="text"
                                class=" p-2 w-full text-gray-500 focus:outline-none" v-model="item.distance">
                            <p v-else>{{ item.distance }}</p>
                        </div>
                        <div class="flex flex-1 flex-col">
                            <!--edit mode-->
                            <label for="duration" class="mb-1 text-sm text-green-700">Duration</label>
                            <input v-if="edit" id="duration" type="text"
                                class=" p-2 w-full text-gray-500 focus:outline-none" v-model="item.duration">
                            <p v-else>{{ item.duration }}</p>
                        </div>
                        <div class="flex flex-1 flex-col">
                            <!--edit mode-->
                            <label for="pace" class="mb-1 text-sm text-green-700">Pace</label>
                            <input v-if="edit" id="pace" type="text"
                                class=" p-2 w-full text-gray-500 focus:outline-none" v-model="item.pace">
                            <p v-else>{{ item.pace }}</p>
                        </div>

                        <img @click="deleteExercise(item.id)" v-if="edit"
                            class="absolute h-4 w-auto -left-5 cursor-pointer"
                            src="~/assets/images/trash-light-green.png" alt="">

                    </div>

                    <button @click="addExcercise" v-if="edit" type="button" class="mt-6 py-2 px-6 max-md:mt-3 max-md:py-1 max-md:px-3 rounded-sm self-start text-sm text-white bg-[#40A578] duration-200 
            border-solid border-2 border-transparent hover:border-[#40A578] hover:bg-white hover:text-[#40A578]">Add
                        excercie</button>
                </div>


            </div>
            <button @click="updateWorkout" v-if="edit" type="button" class="mt-5 py-2 px-6 rounded-sm self-start text-sm text-white bg-[#40A578] duration-200 
            border-solid border-2 border-transparent hover:border-[#40A578] hover:bg-white hover:text-[#40A578]">Update
                Workout</button>


        </div>

    </div>
</template>

<script setup>

definePageMeta({
    middleware: 'auth'
})

const client = useSupabaseClient()
const user = useSupabaseUser()
const { id } = useRoute().params
const router = useRouter()




const data = ref(null) //not made as an array like in index, because only a single row is returned
const dataLoaded = ref(null)
const errorMessage = ref(null)
const statusMessage = ref(null)


//fetch data
const getData = async () => {
    try {
        const { data: workout, error } = await client.from('workouts').select('*').like('userId', user.value.id).eq('id', id)  //eq selects one row, found by id column. first argument is the name of the column, second is the value. and like selects all the rows that contain that particular value inside that column (here being user.id inside userId column), in our case being the users id, this way this data is fetched only when that particular user is logged in. in home we used only eq because it was one conditon, here it is two conditions   
        if (error) throw error //if error fetching data

        if (workout.length < 1) {//if not error fetching data, but error because that row does not exist (or user not authorized based on userId)
            dataLoaded.value = false
            errorMessage.value = "This workout does not exist, or you don't have permission to see this workout."
            return
        }

        data.value = workout[0] //although a single row is returned, but it is returne as array, so use this.
        dataLoaded.value = true
    } catch (error) {
        dataLoaded.value = false
        errorMessage.value = error.message
        setTimeout(() => {
            errorMessage.value = false
        }, 5000);
    }
}
onMounted(() => {
    getData()

})

//edit mode:
const edit = ref(null)
const editMode = () => {
    edit.value = !edit.value
}


//add exercise
const addExcercise = () => {
    if (data.value.workoutType === 'strength') {
        data.value.exercises.push({
            id: Math.floor(Math.random() * 1000000),
            exercise: '',
            reps: '',
            sets: '',
            weight: '',
        })
        return
    }   //else:
    data.value.exercises.push({
        id: Math.floor(Math.random() * 1000000),
        cardioType: '',
        distance: '',
        pace: '',
        duration: ''

    })

}

//delete exercise:
const deleteExercise = (id) => {
    if (data.value.exercises.length > 1) {
        data.value.exercises = data.value.exercises.filter((item) => item.id !== id)
        return
    } //else:
    errorMessage.value = "Error: cannot remove, need to have at least one exercise"
    setTimeout(() => {
        errorMessage.value = false
    }, 5000);
}


//delete workout
const deleteWorkout = async () => {
    try {
        const { error } = await client.from('workouts').delete().eq('id', id) //deletes a row with that id
        if (error) throw error
        router.push('/')
    } catch (error) {
        errorMessage.value = error.message
        setTimeout(() => {
            errorMessage.value = false
        }, 5000);
    }
}

//update workout
const updateWorkout = async () => {
    try {
        const { error } = await client.from('workouts').update({
            workoutName: data.value.workoutName, //these are columns of the row, being updated.
            exercises: data.value.exercises
        }).eq('id', id)//row specified using id
        if (error) throw error
        edit.value = false
        statusMessage.value = "Success: workout updated!"
        setTimeout(() => {
            statusMessage.value = false
        }, 5000);
    } catch (error) {
        errorMessage.value = "Error: " + error.message
        setTimeout(() => {
            errorMessage.value = false
        }, 5000);
    }
}

</script>


<!--when we click on the edit button, we view the input fields and those input fields will show our workout details (workoutName, workoutType, exercises)
that is because they are connected through v-model and v-model is two way data binding.
so in the same div we put both the edit fields and data fields, both connected to the same v-model, so if we change the value
it updates, if not, it just shows the value.-->

<!-- in home page: the data is returned as an array, each row being an item, and the columns being peroperties, so we should place the value in an array. here only one row is returned, so the data variable is not an array.
 in home page, we used v-for because like we said it is an array, here we use v-for only for data.exercises becuase only a single row is returned not an array of rows.
 
-->