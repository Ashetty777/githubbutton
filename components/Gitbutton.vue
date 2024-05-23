<template>
    <v-container class="px-0">
      <div class="github-btn">
        <v-btn
          color="black"
          dark
          medium
          class="text-none"
          @click="logInWithGitHub"
        >
          <v-icon class="mr-2">mdi-github</v-icon>
          <span>Sign in with GitHub</span>
        </v-btn>
      </div>
    </v-container>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue'
  
  const clientID = 'Ov23liI2Ux5E57tRdQiW'; // Replace with your GitHub Client ID
  const redirectURI = 'http://localhost:3000/'; // Replace with your redirect URI
  
  const logInWithGitHub = () => {
    const githubAuthURL = 'https://github.com/login/oauth/authorize?client_id=${clientID}&redirect_uri=${redirectURI}&scope=read:user,user:email';
  
    // Redirect to GitHub authentication page
    window.location.href = githubAuthURL;
  }
  
  const handleGitHubCallback = async () => {
    const urlParams = new URLSearchParams(window.location.search);
    const code = urlParams.get('code');
  
    if (code) {
      try {
        const response = await fetch('https://github.com/login/oauth/access_token', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Accept': 'application/json'
          },
          body: JSON.stringify({
            client_id: clientID,
            client_secret: '1e1ba0e993a73b2d718d6bf1c5e282c2eadec101', // Replace with your GitHub Client Secret
            code
          })
        });
  
        const data = await response.json();
        const accessToken = data.access_token;
  
        // Store the access token in localStorage
        localStorage.setItem('userToken1', accessToken);
        emit('loggedIn');
      } catch (error) {
        console.error('Error during GitHub authentication:', error);
      }
    }
  }
  
  onMounted(() => {
    handleGitHubCallback();
  });
  </script>