<template>
  <div>
    <UCard v-if="!success">
      <template #header>
        Sign-in to Finance Tracker
      </template>

      <form @submit.prevent="handleLogin">
        <UForm label="Email" name="email" class="mb-4" :required="true">
          <UInput type="email" placeholder="Email" v-model="email" />
        </UForm>
        <UButton type="submit" variant="solid" color="primary" :loading="pending" :disabled="pending ">Sign-in</UButton>
      </form>
    </UCard>

    <UCard v-else>
      <template #header>
        Email has been sent
      </template>

      <div class="text-center">
        <p class="mb-4">We have sent and email to <strong>{{ email }}</strong> with a link to sign-in.</p>
        <p>
          <strong>Important:</strong> The link will expire in 5 minutes.
        </p>
      </div>

    </UCard>
  </div>
</template>

<script setup>
const success = ref(false)
const email = ref('')
const pending = ref(false)
const toast = useToast()
const supabase = useSupabaseClient()

useRedirectIfAuthenticated()

const handleLogin = async () => {
  pending.value = true

  try {
    const {error} = await supabase.auth.signInWithOtp({
      email: email.value,
      options: {
        emailRedirectTo: 'http://localhost:3000'
      }
    })
    if(error) {
      toast.add({
        title: 'Error authenticating',
        icon: 'i-heroicoins-exclamation-circle',
        description: error.message,
        color: 'red'
      }) 
    } else {
      success.value = true
    }


  } finally {
    pending.value = false
  }
}

</script>

<style></style>