<script setup lang="ts">
import bgVideo from "../assets/purple-lofi-bedroom-moewalls.com.mp4"
</script>
<template>
    <div class="bgVideo">
        <video autoplay muted loop :src="bgVideo">
        </video>
    </div>
</template>