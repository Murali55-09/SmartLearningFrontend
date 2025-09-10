<template>
  <div class="min-h-screen bg-gray-100 p-6">
    <h1 class="text-3xl font-bold text-center mb-8">Course Categories</h1>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div
        v-for="category in categories"
        :key="category.name"
        class="bg-white rounded-lg shadow p-6"
      >
        <h2 class="text-xl font-semibold mb-4">{{ category.name }}</h2>
        <p class="text-gray-600 mb-4">{{ category.description }}</p>

        <!-- Featured Videos for Category -->
        <div v-if="category.videos && category.videos.length > 0" class="mb-4">
          <h3 class="text-sm font-medium text-gray-700 mb-2">Featured Videos:</h3>
          <div class="grid grid-cols-1 gap-2">
            <div
              v-for="video in category.videos"
              :key="video.id"
              class="flex items-center space-x-3 p-2 bg-gray-50 rounded-lg hover:bg-gray-100 transition"
            >
              <img
                :src="video.thumbnail"
                :alt="video.title"
                class="w-16 h-12 rounded object-cover flex-shrink-0 cursor-pointer"
                @click="openVideo(video.id)"
              />
              <div class="flex-1 min-w-0">
                <p class="text-sm font-medium text-gray-900 truncate cursor-pointer" @click="openVideo(video.id)">{{ video.title }}</p>
              </div>
              <button
                @click="enrollVideo(video, category.name)"
                class="ml-auto bg-green-500 text-white px-3 py-1 rounded hover:bg-green-600 text-sm"
              >
                Enroll
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <h2 class="text-2xl font-bold text-center mb-8 mt-12">Free Engineering Videos</h2>
    <div class="flex justify-center mb-6">
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Search free engineering videos..."
        class="px-4 py-2 rounded-l-lg w-64 focus:outline-none border border-gray-300"
        @keyup.enter="searchVideos"
      />
      <button
        @click="searchVideos"
        class="bg-blue-600 text-white px-6 py-2 rounded-r-lg hover:bg-blue-700 transition"
      >
        Search
      </button>
    </div>

    <!-- Engineering Branch Videos -->
    <div class="mb-8">
      <h3 class="text-xl font-semibold mb-4 text-center">Engineering Branch Videos</h3>
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        <div
          v-for="video in engineeringVideos"
          :key="video.id"
          class="bg-white rounded-lg shadow p-6"
        >
          <h4 class="font-medium">{{ video.title }}</h4>
          <p class="text-sm text-gray-500">{{ video.description }}</p>
          <img
            :src="video.thumbnail"
            :alt="video.title"
            class="w-full mt-2 rounded cursor-pointer object-cover"
            @click="openVideo(video.id)"
          />
        </div>
      </div>
    </div>

    <!-- YouTube API Videos -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div
        v-for="video in freeVideos"
        :key="video.id.videoId"
        class="bg-white rounded-lg shadow p-6"
      >
        <h3 class="font-medium">{{ video.snippet.title }}</h3>
        <p class="text-sm text-gray-500">{{ video.snippet.description }}</p>
        <img
          v-if="video.snippet.thumbnails"
          :src="(video.snippet.thumbnails.high || video.snippet.thumbnails.medium || video.snippet.thumbnails.default).url"
          :alt="video.snippet.title"
          class="w-full mt-2 rounded cursor-pointer object-cover"
          @click="openVideo(video.id.videoId)"
        />
        <div v-else class="w-full mt-2 h-48 bg-gray-200 rounded flex items-center justify-center">
          <span class="text-gray-500">Thumbnail not available</span>
        </div>
      </div>
    </div>

    <h2 class="text-2xl font-bold text-center mb-8 mt-12">Available Courses</h2>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      <div
        v-for="course in courses"
        :key="course.id"
        class="bg-white rounded-lg shadow p-6"
      >
        <h3 class="font-medium">{{ course.title }}</h3>
        <p class="text-sm text-gray-500">{{ course.description }}</p>
        <img
          :src="'https://via.placeholder.com/400x200?text=' + encodeURIComponent(course.title)"
          :alt="course.title"
          class="w-full mt-2 rounded object-cover"
        />
        <button @click="enrollCourse(course.title)" class="bg-blue-600 text-white w-full py-2 rounded-lg hover:bg-blue-700 transition mt-4">Enroll Now</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "CategoryPage",
  data() {
    return {
      categories: [
        {
          name: "Design",
          description: "Master graphic design and UI/UX principles.",
          videos: [
            {
              id: "w7ejDZ8SWv8",
              title: "Adobe Photoshop Tutorial for Beginners",
              thumbnail: "https://img.youtube.com/vi/w7ejDZ8SWv8/maxresdefault.jpg"
            },
            {
              id: "jUjD4iR2JPc",
              title: "Figma UI/UX Design Tutorial",
              thumbnail: "https://img.youtube.com/vi/jUjD4iR2JPc/maxresdefault.jpg"
            }
          ]
        },
        {
          name: "Programming",
          description: "Learn programming languages and web development.",
          videos: [
            {
              id: "rfscVS0vtbw",
              title: "Python Programming Tutorial",
              thumbnail: "https://img.youtube.com/vi/rfscVS0vtbw/maxresdefault.jpg"
            },
            {
              id: "H1elmMBnykA",
              title: "JavaScript Tutorial for Beginners",
              thumbnail: "https://img.youtube.com/vi/H1elmMBnykA/maxresdefault.jpg"
            }
          ]
        },
        {
          name: "Data Science",
          description: "Explore data analysis and machine learning.",
          videos: [
            {
              id: "8jazNUpO3lQ",
              title: "Data Science Tutorial for Beginners",
              thumbnail: "https://img.youtube.com/vi/8jazNUpO3lQ/maxresdefault.jpg"
            },
            {
              id: "HMOI_lkzWZA",
              title: "Machine Learning Tutorial",
              thumbnail: "https://img.youtube.com/vi/HMOI_lkzWZA/maxresdefault.jpg"
            }
          ]
        },
        {
          name: "Business",
          description: "Develop business skills and entrepreneurship.",
          videos: [
            {
              id: "PJiiexo3FuY",
              title: "Business Strategy and Management",
              thumbnail: "https://img.youtube.com/vi/PJiiexo3FuY/maxresdefault.jpg"
            },
            {
              id: "fCn8zs912OE",
              title: "Entrepreneurship Fundamentals",
              thumbnail: "https://img.youtube.com/vi/fCn8zs912OE/maxresdefault.jpg"
            }
          ]
        },
        {
          name: "Marketing",
          description: "Learn digital marketing and advertising.",
          videos: [
            {
              id: "hLwznN9j7gE",
              title: "Digital Marketing Tutorial",
              thumbnail: "https://img.youtube.com/vi/hLwznN9j7gE/maxresdefault.jpg"
            },
            {
              id: "0hEiC9xK1JM",
              title: "Social Media Marketing Guide",
              thumbnail: "https://img.youtube.com/vi/0hEiC9xK1JM/maxresdefault.jpg"
            }
          ]
        }
      ],
      engineeringVideos: [
        {
          id: "rfscVS0vtbw",
          title: "Python Programming Tutorial - Full Course for Beginners",
          description: "Learn Python programming from scratch with this comprehensive tutorial.",
          thumbnail: "https://img.youtube.com/vi/rfscVS0vtbw/maxresdefault.jpg"
        },
        {
          id: "H1elmMBnykA",
          title: "JavaScript Tutorial for Beginners",
          description: "Complete JavaScript course for beginners to learn programming fundamentals.",
          thumbnail: "https://img.youtube.com/vi/H1elmMBnykA/maxresdefault.jpg"
        },
        {
          id: "3JluqTojuME",
          title: "Learn React JS - Full Course for Beginners",
          description: "A full React JS course to build modern web applications.",
          thumbnail: "https://img.youtube.com/vi/3JluqTojuME/maxresdefault.jpg"
        },
        {
          id: "fBNz5xF-Kx4",
          title: "Node.js Tutorial for Beginners",
          description: "Learn Node.js and backend development with this beginner-friendly tutorial.",
          thumbnail: "https://img.youtube.com/vi/fBNz5xF-Kx4/maxresdefault.jpg"
        },
        {
          id: "yfoY53QXEnI",
          title: "CSS Tutorial - Zero to Hero",
          description: "Master CSS with this complete tutorial for beginners.",
          thumbnail: "https://img.youtube.com/vi/yfoY53QXEnI/maxresdefault.jpg"
        },
        {
          id: "Vb0b6v6v6xk",
          title: "Java Programming Tutorial",
          description: "Comprehensive Java programming tutorial for beginners.",
          thumbnail: "https://img.youtube.com/vi/Vb0b6v6v6xk/maxresdefault.jpg"
        }
      ],
      freeVideos: [],
      searchQuery: '',
      apiKey: "AIzaSyAeCxY0aKz9ls9eMuznF4-O-wd_3f5eVhw",
      courses: []
    };
  },
  mounted() {
    this.fetchDefaultVideos();
    this.fetchFreeVideos();
    this.fetchCourses();
  },
  methods: {
    async fetchDefaultVideos() {
      try {
        const response = await axios.get(`https://www.googleapis.com/youtube/v3/search`, {
          params: {
            key: this.apiKey,
            q: 'engineering tutorials',
            type: 'video',
            part: 'snippet',
            maxResults: 3
          }
        });
        this.freeVideos = [...response.data.items, ...this.freeVideos];
      } catch (error) {
        console.error("Error fetching default videos:", error);
      }
    },
    async fetchFreeVideos() {
      try {
        const response = await axios.get(`https://www.googleapis.com/youtube/v3/search`, {
          params: {
            key: this.apiKey,
            q: 'free engineering educational videos',
            type: 'video',
            part: 'snippet',
            maxResults: 6
          }
        });
        // Append to existing videos instead of replacing
        this.freeVideos = [...this.freeVideos, ...response.data.items];
      } catch (error) {
        console.error("Error fetching free videos:", error);
      }
    },
    async searchVideos() {
      if (!this.searchQuery) {
        alert("Please enter a search query.");
        return;
      }
      try {
        const response = await axios.get(`https://www.googleapis.com/youtube/v3/search`, {
          params: {
            key: this.apiKey,
            q: this.searchQuery + ' engineering',
            type: 'video',
            part: 'snippet',
            maxResults: 6
          }
        });
        this.freeVideos = response.data.items;
      } catch (error) {
        console.error("Error searching videos:", error);
        alert("Failed to search videos. Please check your API key.");
      }
    },
    openVideo(videoId) {
      window.open(`https://www.youtube.com/watch?v=${videoId}`, '_blank');
    },
    async fetchCourses() {
      try {
        const response = await axios.get('http://localhost:8080/api/courses');
        this.courses = response.data;
      } catch (error) {
        console.error('Error fetching courses:', error);
      }
    },
    async enrollCourse(courseTitle) {
      const userStr = localStorage.getItem("user");
      if (!userStr) {
        alert("Please log in to enroll in courses.");
        return;
      }

      const user = JSON.parse(userStr);
      const enrollmentData = {
        studentEmail: user.email,
        courseName: courseTitle
      };

      try {
        const response = await fetch("http://localhost:8080/api/enroll", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(enrollmentData)
        });

        if (response.ok) {
          alert(`Successfully enrolled in "${courseTitle}"!`);
          // Optionally refresh the courses list or navigate to dashboard
        } else if (response.status === 409) {
          alert("You are already enrolled in this course.");
        } else {
          alert("Failed to enroll in the course. Please try again.");
        }
      } catch (error) {
        console.error("Error enrolling in course:", error);
        alert("An error occurred while enrolling. Please try again.");
      }
    },
    async enrollVideo(video, categoryName) {
      const userStr = localStorage.getItem("user");
      if (!userStr) {
        alert("Please log in to enroll in videos.");
        return;
      }

      const user = JSON.parse(userStr);
      const enrollmentData = {
        studentEmail: user.email,
        videoTitle: video.title,
        videoId: video.id,
        category: categoryName,
        thumbnail: video.thumbnail
      };

      try {
        const response = await fetch("http://localhost:8080/api/video-enroll", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(enrollmentData)
        });

        if (response.ok) {
          alert(`Successfully enrolled in "${video.title}"!`);
        } else if (response.status === 400) {
          alert("You are already enrolled in this video.");
        } else {
          alert("Failed to enroll in the video. Please try again.");
        }
      } catch (error) {
        console.error("Error enrolling in video:", error);
        alert("An error occurred while enrolling. Please try again.");
      }
    }
  }
};
</script>

<style scoped>
/* Add any custom styles if needed */
</style>
