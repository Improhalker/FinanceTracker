<template>
    <header class="flex justify-between items-center mt-10">
      <NuxtLink to="/" class="text-xl font-bold">
        Finance Tracker
      </NuxtLink>
      <div>
        <UAvatar src="https://avatars.githubusercontent.com/u/739984?v=4" alt="Avatar" />
        <ColorModeSelector />
      </div>
    </header>
</template>

<script setup>

import { useRouter } from 'vue-router'
const user = useSupabaseUser()
const router = useRouter()
watch(user, (user) => {
  if(user) {
    console.log('Signed in!')
  } else {
    console.log('Not logged')
    router.push('/login')
    
  }
}, {immediate: true})

</script>