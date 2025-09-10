<template>
  <div class="font-sans text-gray-800">
    <!-- Hero / Banner Section -->
    <section class="bg-gradient-to-r from-blue-500 to-indigo-600 text-white py-20 px-6">
      <div class="container mx-auto text-center">
        <h1 class="text-4xl md:text-5xl font-bold mb-4">Learn Anything, Anytime</h1>
        <p class="mb-6 text-lg md:text-xl">Explore thousands of courses online from expert instructors.</p>
        <div class="flex justify-center space-x-4 mb-6">
          <input v-model="searchQuery" type="text" placeholder="Search courses..." class="px-4 py-2 rounded-l-lg w-64 focus:outline-none text-gray-800 bg-white"/>
          <button @click="searchCourses" class="bg-white text-blue-600 font-bold px-6 rounded-r-lg hover:bg-gray-100 transition">Search</button>
        </div>
        <!-- Search Results -->
        <div v-if="searchResults.length > 0" class="mb-6">
          <h3 class="text-xl font-bold mb-4">Search Results</h3>
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
            <div v-for="course in searchResults" :key="course.id" class="bg-white shadow-lg rounded-lg p-4 hover:shadow-xl transition">
              <h4 class="font-bold text-lg mb-2">{{ course.title }}</h4>
              <p class="text-gray-600">{{ course.description }}</p>
              <button @click="enrollCourse(course.title)" class="bg-blue-600 text-white w-full py-2 rounded-lg hover:bg-blue-700 transition">Enroll Now</button>
            </div>
          </div>
        </div>
        <div v-if="searchQuery && searchResults.length === 0 && hasSearched" class="mb-6 text-center text-gray-600">
          No courses found for "{{ searchQuery }}"
        </div>
        <div class="space-x-4">
          <button class="bg-white text-blue-600 font-bold px-6 py-2 rounded-lg hover:bg-gray-100 transition">Explore Courses</button>
          <button class="bg-blue-800 text-white font-bold px-6 py-2 rounded-lg hover:bg-blue-900 transition">Start Learning</button>
        </div>
      </div>
    </section>

    <!-- Popular Categories -->
    <section class="py-20 px-6">
      <div class="container mx-auto text-center mb-10">
        <h2 class="text-3xl font-bold mb-4">Popular Categories</h2>
        <p class="text-gray-600">Explore courses by categories</p>
      </div>
      <div class="grid grid-cols-2 md:grid-cols-4 gap-6 container mx-auto">
        <div v-for="category in categories" :key="category.name" class="bg-white shadow-lg rounded-lg p-6 hover:shadow-xl transition cursor-pointer" @click="$router.push('/categories')">
          <img :src="category.image" alt="" class="w-16 h-16 mx-auto mb-4"/>
          <h3 class="font-bold text-lg">{{ category.name }}</h3>
          <p class="text-gray-500">{{ category.courses }} Courses</p>
        </div>
      </div>
    </section>

    <!-- Trending Courses -->
    <section class="bg-gray-100 py-20 px-6">
      <div class="container mx-auto text-center mb-10">
        <h2 class="text-3xl font-bold mb-4">Trending Courses</h2>
        <p class="text-gray-600">Most popular courses among students</p>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6 container mx-auto">
        <div v-for="course in courses" :key="course.id" class="bg-white shadow-lg rounded-lg p-6 hover:shadow-xl transition">
          <img :src="'https://via.placeholder.com/400x200?text=' + encodeURIComponent(course.title)" alt="" class="w-full h-40 object-cover rounded-lg mb-4"/>
          <h3 class="font-bold text-lg mb-1">{{ course.title }}</h3>
          <p class="text-gray-500 mb-2">{{ course.description }}</p>
          <button @click="enrollCourse(course.title)" class="bg-blue-600 text-white w-full py-2 rounded-lg hover:bg-blue-700 transition">Enroll Now</button>
        </div>
      </div>
    </section>

    <!-- Why Choose Us -->
    <section class="py-20 px-6">
      <div class="container mx-auto text-center mb-10">
        <h2 class="text-3xl font-bold mb-4">Why Choose Us</h2>
        <p class="text-gray-600">Learn from the best with flexible and lifetime access</p>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-4 gap-6 container mx-auto text-center">
        <div v-for="benefit in benefits" :key="benefit.title" class="p-6 bg-white shadow-lg rounded-lg hover:shadow-xl transition">
          <img :src="benefit.icon" alt="" class="w-12 h-12 mx-auto mb-4"/>
          <h3 class="font-bold text-lg mb-2">{{ benefit.title }}</h3>
          <p class="text-gray-500">{{ benefit.description }}</p>
        </div>
      </div>
    </section>

    <!-- Login/Register Alert Modal -->
    <div v-if="showModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
      <div class="bg-white rounded-lg p-6 max-w-md w-full mx-4 alert-modal">
        <h3 class="text-xl font-bold mb-4 text-center">Welcome!</h3>
        <p class="text-gray-600 mb-6 text-center">Please login or register to access all features.</p>
        <div class="flex justify-center space-x-4">
          <button @click="goToLogin" class="bg-blue-600 text-white px-6 py-2 rounded-lg hover:bg-blue-700 transition">Login</button>
          <button @click="goToRegister" class="bg-green-600 text-white px-6 py-2 rounded-lg hover:bg-green-700 transition">Register</button>
        </div>
        <button @click="closeModal" class="absolute top-2 right-2 text-gray-500 hover:text-gray-700">&times;</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HomePage",
  data() {
    return {
      showModal: true,
      searchQuery: '',
      searchResults: [],
      hasSearched: false,
      categories: [
        { name: "Web Development", courses: 120, image: "https://images.unsplash.com/photo-1581091012184-6c3508d79d2f?auto=format&fit=crop&w=80&q=80" },
        { name: "Data Science", courses: 85, image: "https://images.unsplash.com/photo-1517430816045-df4b7de11d1f?auto=format&fit=crop&w=80&q=80" },
        { name: "Design", courses: 60, image: "https://images.unsplash.com/photo-1509228627152-0e04d62f5e0f?auto=format&fit=crop&w=80&q=80" },
        { name: "Marketing", courses: 45, image: "https://images.unsplash.com/photo-1504384308090-c894fdcc538d?auto=format&fit=crop&w=80&q=80" }
      ],
      courses: [],
      benefits: [
        { title: "Expert Instructors", description: "Learn from industry experts", icon: "https://img.icons8.com/color/48/000000/teacher.png" },
        { title: "Lifetime Access", description: "Access courses anytime", icon: "https://img.icons8.com/color/48/000000/infinity.png" },
        { title: "Certificates", description: "Get recognized certificates", icon: "https://img.icons8.com/color/48/000000/certificate.png" },
        { title: "Flexible Learning", description: "Learn at your own pace", icon: "https://img.icons8.com/color/48/000000/flexible.png" }
      ]
    };
  },
  mounted() {
    // Modal shows on page load
  },
  methods: {
    async searchCourses() {
      if (!this.searchQuery.trim()) {
        this.searchResults = [];
        this.hasSearched = false;
        return;
      }

      try {
        const response = await fetch(`http://localhost:8080/api/courses/search?query=${encodeURIComponent(this.searchQuery)}`);
        if (response.ok) {
          this.searchResults = await response.json();
        } else {
          console.error('Search failed:', response.statusText);
          this.searchResults = [];
        }
      } catch (error) {
        console.error('Error searching courses:', error);
        this.searchResults = [];
      }
      this.hasSearched = true;
    },
    async enrollCourse(courseTitle) {
      try {
        // Assuming studentEmail is available in localStorage or some auth state
        const studentEmail = localStorage.getItem('studentEmail') || 'test@example.com';
        const response = await fetch('http://localhost:8080/api/enroll', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ studentEmail, courseName: courseTitle })
        });
        if (response.ok) {
          alert(`Successfully enrolled in ${courseTitle}`);
        } else {
          alert('Failed to enroll in course');
        }
      } catch (error) {
        console.error('Error enrolling in course:', error);
        alert('Error enrolling in course');
      }
    },
    closeModal() {
      this.showModal = false;
    },
    goToLogin() {
      this.$router.push('/login');
    },
    goToRegister() {
      this.$router.push('/register');
    }
  }
};
</script>

<style scoped>
.container { max-width: 1200px; }
</style>

<style scoped>
.alert-modal {
  animation: pulseAlert 1.5s infinite;
  position: relative;
}

@keyframes pulseAlert {
  0% {
    box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.7);
  }
  70% {
    box-shadow: 0 0 0 10px rgba(239, 68, 68, 0);
  }
  100% {
    box-shadow: 0 0 0 0 rgba(239, 68, 68, 0);
  }
}
</style>
