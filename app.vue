<template>
  <div>
    <h1>Deploy to Cloudflare Workers</h1>
    <button @click="triggerDeploy">Trigger Deployment</button>
    <p>{{ status }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// State for displaying status messages
const status = ref('')

// GitHub repository details
const GITHUB_REPO = "devops-dappgenie/demo" // Format: username/repo-name
const GITHUB_TOKEN = "your-personal-access-token" // Replace with your GitHub PAT

const triggerDeploy = async () => {
  const apiUrl = `https://api.github.com/repos/${GITHUB_REPO}/actions/workflows/main.yml/dispatches`
  status.value = "Triggering deployment..."

  try {
    const response = await fetch(apiUrl, {
      method: "POST",
      headers: {
        "Authorization": `Bearer ${GITHUB_TOKEN}`,
        "Accept": "application/vnd.github.v3+json",
        "Content-Type": "application/json"
      },
      body: JSON.stringify({
        ref: "main" // Trigger workflow on the main branch
        // No inputs field here
      })
    })

    if (response.ok) {
      status.value = "Deployment triggered successfully!"
    } else {
      const errorText = await response.text()
      status.value = `Error: ${errorText}`
    }
  } catch (error) {
    status.value = `Error: ${error.message}`
  }
}
</script>

<style>
button {
  padding: 10px 20px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #2980b9;
}
</style>
