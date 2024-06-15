<template>
    <header class="bg-[#40A578] text-white">
        <nav class="w-[80%] mx-auto py-5 px-4 flex flex-col sm:flex-row justify-center items-center gap-4">
            <div class="flex items-center gap-x-4">
                <img class="w-14" src="~/assets/images/dumbbell-light.png" alt="">
                <h1 class="text-lg">Active Tracker</h1>
            </div>
            <ul class="flex flex-1 justify-end gap-x-10">
                <NuxtLink class=" cursor-pointer" to="/">Home</NuxtLink>
                <NuxtLink class=" cursor-pointer" to="/create" v-if="user">Create</NuxtLink>
                <NuxtLink class=" cursor-pointer" to="/login" v-if="!user">Login</NuxtLink>
                <li class=" cursor-pointer" @click="logout" v-if="user">Logout</li>
            </ul>
        </nav>
    </header>

    <slot />
    <div v-if="errorMessage"  class="text-red-500 max-w-max mx-auto mt-10 text-4xl">Error: {{  }}</div>
</template>

<script setup>
const user = useSupabaseUser()
const router = useRouter()
const client = useSupabaseClient()
const errorMessage = ref(null)

const logout = async () => {
    try {
        const { error } = await client.auth.signOut()

        if (error) throw error
        router.push('/')
    } catch (error) {
        errorMessage.value = error.value
    }
}




</script>
