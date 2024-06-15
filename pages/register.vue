<template>
    <Head>
        <Title>Register</Title>
    </Head>
    <div class=" max-w-screen-sm mx-auto px-4 py-10 max-md:px-2 max-md:py-5">
        <div v-if="errorMessage" class=" mb-10 max-md:mb-5 rounded-md shadow-md bg-[#f1f1f1] p-4">
            <p class="text-red-500">{{ errorMessage }}</p>
        </div>
        <form @submit.prevent="register" class="p-8 max-md:p-4 flex flex-col bg-[#f1f1f1] rounded-md shadow-lg">
            <h1 class="text-3xl max-md:text-xl text-[#40A578] mb-4">Register</h1>
            <div class="flex flex-col mb-2">
                <label for="email" class="mb-1 text-sm text-green-700">Email</label>
                <input type="text" required class="p-2 max-md:p-1 text-gray-500 focus:outline-none" id="email" v-model="email">
            </div>
            <div class="flex flex-col mb-2">
                <label for="password" class="mb-1 text-sm text-[#40A578]">Password</label>
                <input type="password" required class="p-2 max-md:p-1 text-gray-500 focus:outline-none" id="password"
                    v-model="password">
            </div>
            <div class="flex flex-col mb-2">
                <label for="confirmPassword" class="mb-1 text-sm text-[#40A578]">Confirm Password</label>
                <input type="password" required class="p-2 max-md:p-1 text-gray-500 focus:outline-none" id="confirmPassword"
                    v-model="confirmPassword">
            </div>
            <button type="submit"
                class="mt-6 py-2 px-6 max-md:mt-3 max-md:py-1 max-md:px-3 rounded-sm self-start text-sm text-white bg-[#40A578] duration-200 
            border-solid border-2 border-transparent hover:border-[#40A578] hover:bg-white hover:text-[#40A578]">Register</button>
            <NuxtLink class=" text-sm mt-6 max-md:mt-3 text-center" to="/login">Already have an account? <span
                    class="text-[#40A578]">Login</span></NuxtLink>
        </form>
    </div>


</template>

<script setup>
const email = ref(null)
const password = ref(null)
const confirmPassword = ref(null)
const errorMessage = ref(null)

const client = useSupabaseClient()
const router = useRouter()

const register = async () => {
    if (password.value === confirmPassword.value) {
        try {
            const { error } = await client.auth.signUp({
                email: email.value,
                password: password.value,
            })
            if (error) throw error
            router.push('/login')
        } catch (error) {
            errorMessage.value = error.message
            setTimeout(() => {
                errorMessage.value = null
            }, 5000);
        }
        return //prevents the code below from executing 
    }
    errorMessage.value = "Error: passwords don't match"
    setTimeout(() => {
        errorMessage.value = null
    }, 5000);
}
</script>
