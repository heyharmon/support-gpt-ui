<template>
  <NuxtLayout>
    <h1 class="text-xl font-bold mb-4">Ask a question</h1>

    <div v-if="loading">Getting an answer...</div>

    <div v-else class="mb-8">
      <input v-model="prompt" @keyup.enter="submit()" type="text" name="prompt" id="prompt" class="block mb-2 w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" placeholder="Type your question here..." />
      <button v-if="prompt" @click="submit()" type="button" class="w-full rounded-md bg-indigo-600 px-2.5 py-1.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Submit</button>

      <div v-if="answer" class="mt-6 rounded-lg bg-gray-50 p-4">
        <h2 class="text-lg font-bold mb-2">Answer:</h2>
        <div v-html="markdownToHtml"></div>
      </div>

    </div>

    <hr class="my-4">
    
      <p class="text-gray-500 font-bold mb-1">Sample prompts</p>
      <ul class="text-gray-500 list-disc list-inside mb-3">
        <!-- <li>What font filetype do we need for the website?</li>
        <li>How does bloomcu secure a wordpress theme?</li>
        <li>Is a button color change something that is in-scope?</li>
        <li>How could the context be improved for the following question: [insert your question]?</li> -->

        <li>I want to implement a chat window on my website, can you set this up?</li>
        <li>A third party widget on our website is not working. Can you fix it?</li>
        <li>We need to update the call to action “Apply now” and “Open account” across our website.</li>
      </ul>

      <!-- <p class="text-gray-500 mb-1 italic">What font filetype do we need for the website?</p>
      <p class="text-gray-500 mb-1 italic">How does bloomcu secure a wordpress theme?</p>
      <p class="text-gray-500 mb-1 italic">Is a button color change something that is in-scope?</p>
      <p class="text-gray-500 mb-3 italic">How could the context be improved for the following question: [insert your question]?</p> -->
      
      <hr class="my-4">

      <p class="text-gray-500 font-bold mb-1">About</p>
      <!-- <p class="text-gray-500">This Ai is trained on 200 of our Nuclino articles that start with "How", "What", "Where" consolidated into one <a class="underline text-indigo-600" href="https://docs.google.com/document/d/1qmG5KtGh7eanbgM_2fHagz11uhJ-USlHiZQs1nlSJxA/edit?usp=sharing">Google Doc</a>. The training data format is subpar because comes in markdown format from Nuclino. The article titles are missing as well. Despite this, the Ai is able to provide useful responses.</p> -->
      <p class="text-gray-500">This Ai is trained on <a class="underline text-indigo-600" href="https://docs.google.com/document/d/1BuQ_WQCNZiDOaJ9C-yO55dNWZ_bhWUy9UJvn2LWTH_8/edit?usp=sharing">How do I know if I should bill a client for a support request or not? (Is the request in-scope or out-of-scope for a Support subscription?)</a>.</p>
  </NuxtLayout>
</template>

<script setup>
import { ref } from 'vue'
import { marked } from 'marked'

let loading = ref(false)
let prompt = ref('')
let answer = ref('')

async function submit() {
    if (prompt === '') return
    loading.value = true

    const response = await fetch(`https://support-gpt-api.herokuapp.com?prompt=${prompt.value}`)
    const data = await response.json()
    
    prompt.value = ''
    answer.value = data.message.response
    loading.value = false
}

let markdownToHtml = computed(() => {
  return marked(answer.value);
})
</script>