<template>
  <div class="min-h-screen bg-gray-100">
    <!-- Header -->
    <div class="bg-white shadow">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between items-center py-6">
          <h1 class="text-3xl font-bold text-gray-900">Admin Dashboard</h1>
          <button @click="logout" class="bg-red-600 hover:bg-red-700 text-white px-4 py-2 rounded-md">
            Logout
          </button>
        </div>
      </div>
    </div>

    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <!-- Statistics Cards -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
        <div class="bg-white overflow-hidden shadow rounded-lg">
          <div class="p-5">
            <div class="flex items-center">
              <div class="flex-shrink-0">
                <svg class="h-8 w-8 text-blue-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197m13.5-9a2.5 2.5 0 11-5 0 2.5 2.5 0 015 0z"></path>
                </svg>
              </div>
              <div class="ml-5 w-0 flex-1">
                <dl>
                  <dt class="text-sm font-medium text-gray-500 truncate">Total Students</dt>
                  <dd class="text-lg font-medium text-gray-900">{{ students.length }}</dd>
                </dl>
              </div>
            </div>
          </div>
        </div>

        <div class="bg-white overflow-hidden shadow rounded-lg">
          <div class="p-5">
            <div class="flex items-center">
              <div class="flex-shrink-0">
                <svg class="h-8 w-8 text-green-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.746 0 3.332.477 4.5 1.253v13C19.832 18.477 18.246 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"></path>
                </svg>
              </div>
              <div class="ml-5 w-0 flex-1">
                <dl>
                  <dt class="text-sm font-medium text-gray-500 truncate">Total Courses</dt>
                  <dd class="text-lg font-medium text-gray-900">{{ courses.length }}</dd>
                </dl>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Students Management -->
      <div class="bg-white shadow rounded-lg mb-8">
        <div class="px-4 py-5 sm:p-6">
          <h3 class="text-lg leading-6 font-medium text-gray-900 mb-4">Students Management</h3>

          <!-- Add Student Form -->
          <div class="mb-6">
            <h4 class="text-md font-medium text-gray-900 mb-2">Add New Student</h4>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
              <input v-model="newStudent.name" type="text" placeholder="Name" class="border border-gray-300 rounded-md px-3 py-2">
              <input v-model="newStudent.email" type="email" placeholder="Email" class="border border-gray-300 rounded-md px-3 py-2">
              <input v-model="newStudent.password" type="password" placeholder="Password" class="border border-gray-300 rounded-md px-3 py-2">
            </div>
            <button @click="addStudent" class="mt-2 bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-md">
              Add Student
            </button>
          </div>

          <!-- Students List -->
          <div>
            <h4 class="text-md font-medium text-gray-900 mb-2">All Students</h4>
            <div class="overflow-x-auto">
              <table class="min-w-full divide-y divide-gray-200">
                <thead class="bg-gray-50">
                  <tr>
                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">ID</th>
                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Name</th>
                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Email</th>
                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
                  </tr>
                </thead>
                <tbody class="bg-white divide-y divide-gray-200">
                  <tr v-for="student in students" :key="student.id">
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">{{ student.id }}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">{{ student.name }}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">{{ student.email }}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium">
                      <button @click="deleteStudent(student.id)" class="text-red-600 hover:text-red-900">Delete</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>

      <!-- Courses Management -->
      <div class="bg-white shadow rounded-lg">
        <div class="px-4 py-5 sm:p-6">
          <h3 class="text-lg leading-6 font-medium text-gray-900 mb-4">Courses Management</h3>

          <!-- Add Course Form -->
          <div class="mb-6">
            <h4 class="text-md font-medium text-gray-900 mb-2">Add New Course</h4>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <input v-model="newCourse.title" type="text" placeholder="Course Title" class="border border-gray-300 rounded-md px-3 py-2">
              <input v-model="newCourse.description" type="text" placeholder="Course Description" class="border border-gray-300 rounded-md px-3 py-2">
            </div>
            <button @click="addCourse" class="mt-2 bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-md">
              Add Course
            </button>
          </div>

          <!-- Courses List -->
          <div>
            <h4 class="text-md font-medium text-gray-900 mb-2">All Courses</h4>
            <div class="overflow-x-auto">
              <table class="min-w-full divide-y divide-gray-200">
                <thead class="bg-gray-50">
                  <tr>
                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">ID</th>
                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Title</th>
                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Description</th>
                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
                  </tr>
                </thead>
                <tbody class="bg-white divide-y divide-gray-200">
                  <tr v-for="course in courses" :key="course.id">
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">{{ course.id }}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">{{ course.title }}</td>
                    <td class="px-6 py-4 text-sm text-gray-500">{{ course.description }}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium">
                      <button @click="deleteCourse(course.id)" class="text-red-600 hover:text-red-900">Delete</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AdminDashboard",
  data() {
    return {
      students: [],
      courses: [],
      newStudent: {
        name: '',
        email: '',
        password: ''
      },
      newCourse: {
        title: '',
        description: ''
      }
    };
  },
  mounted() {
    this.fetchStudents();
    this.fetchCourses();
  },
  methods: {
    async fetchStudents() {
      try {
        const response = await fetch('http://localhost:8080/api/users');
        if (response.ok) {
          this.students = await response.json();
        }
      } catch (error) {
        console.error('Error fetching students:', error);
      }
    },

    async fetchCourses() {
      try {
        const response = await fetch('http://localhost:8080/api/courses');
        if (response.ok) {
          this.courses = await response.json();
        }
      } catch (error) {
        console.error('Error fetching courses:', error);
      }
    },

    async addStudent() {
      if (!this.newStudent.name || !this.newStudent.email || !this.newStudent.password) {
        alert('Please fill in all fields');
        return;
      }

      try {
        const response = await fetch('http://localhost:8080/api/auth/register', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            name: this.newStudent.name,
            email: this.newStudent.email,
            password: this.newStudent.password,
            role: 'STUDENT'
          }),
        });

        if (response.ok) {
          alert('Student added successfully!');
          this.newStudent = { name: '', email: '', password: '' };
          this.fetchStudents();
        } else {
          alert('Failed to add student');
        }
      } catch (error) {
        console.error('Error adding student:', error);
        alert('Error adding student');
      }
    },

    async addCourse() {
      if (!this.newCourse.title || !this.newCourse.description) {
        alert('Please fill in all fields');
        return;
      }

      try {
        const response = await fetch('http://localhost:8080/api/courses', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(this.newCourse),
        });

        if (response.ok) {
          alert('Course added successfully!');
          this.newCourse = { title: '', description: '' };
          this.fetchCourses();
        } else {
          alert('Failed to add course');
        }
      } catch (error) {
        console.error('Error adding course:', error);
        alert('Error adding course');
      }
    },

    async deleteStudent(id) {
      if (!confirm('Are you sure you want to delete this student?')) {
        return;
      }

      try {
        const response = await fetch(`http://localhost:8080/api/users/${id}`, {
          method: 'DELETE',
        });

        if (response.ok) {
          alert('Student deleted successfully!');
          this.fetchStudents();
        } else {
          alert('Failed to delete student');
        }
      } catch (error) {
        console.error('Error deleting student:', error);
        alert('Error deleting student');
      }
    },

    async deleteCourse(id) {
      if (!confirm('Are you sure you want to delete this course?')) {
        return;
      }

      try {
        const response = await fetch(`http://localhost:8080/api/courses/${id}`, {
          method: 'DELETE',
        });

        if (response.ok) {
          alert('Course deleted successfully!');
          this.fetchCourses();
        } else {
          alert('Failed to delete course');
        }
      } catch (error) {
        console.error('Error deleting course:', error);
        alert('Error deleting course');
      }
    },

    logout() {
      localStorage.removeItem("userRole");
      this.$router.push("/login");
    },
  },
};
</script>
