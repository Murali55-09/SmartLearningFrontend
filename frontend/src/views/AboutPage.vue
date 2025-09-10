<template>
  <div class="min-h-screen bg-gray-100 p-6">
    <div class="max-w-4xl mx-auto bg-white rounded-lg shadow p-8">
      <h1 class="text-4xl font-bold text-center mb-8">About SmartLearn</h1>
      <div class="prose prose-lg mx-auto">
        <p class="text-gray-700 mb-6">
          Welcome to SmartLearn, your premier destination for online learning and skill development.
          We are committed to providing high-quality educational content to learners worldwide.
        </p>
        <h2 class="text-2xl font-semibold mb-4">Our Mission</h2>
        <p class="text-gray-700 mb-6">
          Our mission is to democratize education by making learning accessible, affordable, and engaging
          for everyone, regardless of their background or location.
        </p>
        <h2 class="text-2xl font-semibold mb-4">What We Offer</h2>
        <ul class="list-disc list-inside text-gray-700 mb-6">
          <li>Comprehensive course catalog covering various subjects</li>
          <li>Expert instructors with real-world experience</li>
          <li>Interactive learning materials and assessments</li>
          <li>Flexible learning schedules</li>
          <li>Community support and networking opportunities</li>
        </ul>
        <h2 class="text-2xl font-semibold mb-4">Our Team</h2>
        <p class="text-gray-700 mb-6">
          Our dedicated team consists of educators, technologists, and industry professionals
          who are passionate about transforming the way people learn.
        </p>
        <h2 class="text-2xl font-semibold mb-4">Contact Us</h2>
        <p class="text-gray-700">
          Have questions or feedback? We'd love to hear from you.
          Visit our <router-link to="/contact" class="text-blue-600 hover:underline">Contact page</router-link>
          to get in touch.
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AboutPage"
};
</script>

<style scoped>
.prose {
  max-width: none;
}
</style>
