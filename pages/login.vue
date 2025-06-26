<template>
  <div>
    <UCard v-if="!success">
      <template #header>
        Sign-in to Finance Tracker
      </template>

      <form action="">
        <UForm label="Email" name="email" class="mb-4" :required="true">
          <UInput type="email" placeholder="Email" />
        </UForm>
        <UButton type="submit" variant="solid" @click="success = true" color="primary">Sign-in</UButton>
      </form>
    </UCard>

    <UCard v-else>
      <template #header>
        Email has been sent
      </template>

      <div class="text-center">
        <p class="mb-4">We have sent and email to <strong>gabrielestrela@email.com</strong> with a link to sign-in.</p>
        <p>
          <strong>Important:</strong> The link will expire in 5 minutes.
        </p>
      </div>

    </UCard>
  </div>
</template>

<script setup>
const success = ref(false)
</script>

<style></style>