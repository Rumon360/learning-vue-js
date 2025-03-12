<script setup>
import Navbar from "@/components/navbar.vue";
import Hero from "@/components/hero.vue";
import HomeCards from "@/components/home-cards.vue";
import JobListings from "@/components/job-listings.vue";
</script>

<template>
  <Navbar />
  <Hero subtitle="Find the Vue job that fits your skills and needs" />
  <HomeCards />
  <JobListings />
</template>
