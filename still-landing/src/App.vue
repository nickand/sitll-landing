<script setup lang="ts">
import { ref } from 'vue'

const email = ref('')
const isSubmitting = ref(false)
const submitted = ref(false)
const error = ref('')

// 👇 Pon aquí tu webhook de Make / backend / lo que uses
const WEBHOOK_URL = 'https://hook.us2.make.com/tibjcootr0umiq97tv5k593bw6p3nciy'

const handleSubmit = async () => {
  error.value = ''

  if (!email.value.trim()) {
    error.value = 'Please enter a valid email.'
    return
  }

  isSubmitting.value = true

  try {
    await fetch(WEBHOOK_URL, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ email: email.value.trim() }),
    })

    submitted.value = true
    email.value = ''
  } catch (e) {
    console.error(e)
    error.value = 'Something went wrong. Please try again.'
  } finally {
    isSubmitting.value = false
  }
}
</script>

<template>
  <div class="page">
    <header class="nav">
      <div class="logo">
        <img src="./assets/wave.png" alt="Still Logo" class="logo-img" />
        <span class="logo-text">still</span>
      </div>

    </header>

    <main class="hero">
      <section class="hero-content">
        <p class="tagline">A private space to think.</p>

        <h1 class="headline">
          Where your thoughts<br />
          can breathe.
        </h1>

        <p class="subcopy">
          Still is a quiet place to write, reflect, and reset your mind.
          No feeds, no likes, no noise — just you and your thoughts.
        </p>

        <form class="form" @submit.prevent="handleSubmit">
          <div class="input-wrapper" v-if="!submitted">
            <input
              v-model="email"
              type="email"
              placeholder="Enter your email"
              class="input"
            />
            <button class="button" type="submit" :disabled="isSubmitting">
              <span v-if="!isSubmitting">Join the waitlist</span>
              <span v-else>Sending…</span>
            </button>
          </div>

          <p v-if="submitted" class="success">
            You’re in. I’ll let you know when Still is ready.
          </p>

          <p v-if="error" class="error">
            {{ error }}
          </p>

          <p class="helper">
            No spam. Just launch updates and a few quiet ideas.
          </p>
        </form>
      </section>

      <section class="hero-card">
        <div class="card">
          <img src="./assets/wave.png" alt="Still Logo" class="card-logo" />
          <p class="card-title">Where your thoughts can breathe.</p>
          <p class="card-sub">Write without noise.</p>
        </div>
      </section>
    </main>

    <footer class="footer">
      <span>© {{ new Date().getFullYear() }} Still</span>
      <span>Built quietly by @justnickand</span>
    </footer>
  </div>
</template>
