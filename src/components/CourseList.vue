<template>
  <div class="container mt-5">
    <h2 class="text-center text-light">Course List</h2>
    <ul class="list-group">
      <li v-for="course in courses" :key="course.id" class="list-group-item d-flex justify-content-between align-items-center">
        <div>
          <strong>{{ course.code }}:</strong> {{ course.name }} - {{ course.credits }} credits - Grade: {{ course.grade }}
        </div>
        <div>
          <button class="btn btn-warning me-2" @click="editCourse(course)">Edit</button>
          <button class="btn btn-danger" @click="deleteCourse(course.id)">Delete</button>
        </div>
      </li>
    </ul>

    <!-- ปุ่มเพิ่มรายวิชา -->
    <button class="btn btn-success mt-3" @click="showForm = true">Add New Course</button>

    <!-- ปุ่มกลับ -->
    <router-link to="/profile" class="btn btn-light mt-3 ms-2">
  <img src="https://cdn-icons-png.flaticon.com/32/10294/10294919.png" alt="Back Icon" width="20" height="20" class="me-1"></router-link>

    <router-link to="/" class="btn btn-light mt-3 ms-2">
  <img src="https://cdn-icons-png.flaticon.com/32/3917/3917032.png" alt="Home Icon" width="20" height="20" class="me-1"></router-link>
    <!-- Form สำหรับเพิ่ม/แก้ไขรายวิชา -->
    <div v-if="showForm" class="mt-4">
      <h3 class="text-center text-light">{{ isEditing ? 'Edit Course' : 'Add New Course' }}</h3>
      <form @submit.prevent="saveCourse">
        <input v-model="course.code" class="form-control mb-2" placeholder="Course Code" required />
        <input v-model="course.name" class="form-control mb-2" placeholder="Course Name" required />
        <input v-model="course.credits" type="number" class="form-control mb-2" placeholder="Credits" required />
        <input v-model="course.grade"  class="form-control mb-2" placeholder="Grade" required />
        <button type="submit" class="btn btn-primary">{{ isEditing ? 'Save Changes' : 'Add Course' }}</button>
        <button type="button" class="btn btn-secondary ms-2" @click="cancelEdit">Cancel</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      courses: [],
      showForm: false,
      isEditing: false,
      course: { id: "", code: "", name: "", credits: "", grade: "" }
    };
  },
  async mounted() {
    await this.fetchCourses();
  },
  methods: {
    async fetchCourses() {
      try {
        const response = await fetch("http://localhost:3000/courses");
        this.courses = await response.json();
      } catch (error) {
        console.error("Error fetching courses:", error);
      }
    },
    async deleteCourse(id) {
      try {
        await fetch(`http://localhost:3000/courses/${id}`, { method: "DELETE" });

        // ลบข้อมูลออกจาก UI ทันที
        this.courses = this.courses.filter((course) => course.id !== id);
      } catch (error) {
        console.error("Error deleting course:", error);
      }
    },
    editCourse(course) {
      this.course = { ...course }; // ก๊อปปี้ข้อมูลมาแก้ไข
      this.isEditing = true;
      this.showForm = true;
    },
    async saveCourse() {
      try {
        if (this.isEditing) {
          await fetch(`http://localhost:3000/courses/${this.course.id}`, {
            method: "PUT",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
              id: String(this.course.id),
              code: String(this.course.code),
              name: String(this.course.name),
              credits: String(Number(this.course.credits)), // credits เป็น string (แต่รับค่าตัวเลข)
              grade: String(this.course.grade)
            })
          });

          // อัปเดตข้อมูลในหน้าเว็บทันที
          const index = this.courses.findIndex((c) => c.id === this.course.id);
          if (index !== -1) {
            this.courses[index] = { ...this.course };
          }
        } else {
          const newId = this.courses.length > 0 ? String(Math.max(...this.courses.map((c) => Number(c.id))) + 1) : "1";

          const newCourse = {
            id: newId,
            code: String(this.course.code),
            name: String(this.course.name),
            credits: String(Number(this.course.credits)), // credits เป็น string (แต่รับค่าตัวเลข)
            grade: String(this.course.grade)
          };

          await fetch("http://localhost:3000/courses", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(newCourse)
          });

          // เพิ่มข้อมูลใหม่เข้า UI ทันที
          this.courses.push(newCourse);
        }

        this.cancelEdit(); // ปิดฟอร์ม
      } catch (error) {
        console.error("Error saving course:", error);
      }
    },
    cancelEdit() {
      this.showForm = false; // ปิดฟอร์ม
      this.isEditing = false;
      this.course = { id: "", code: "", name: "", credits: "", grade: "" }; // ล้างค่า
    }
  }
};
</script>

<style scoped>
.container {
  background-color: #2d3436;
  padding: 30px;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

h2, h3 {
  font-family: 'Playfair Display', serif;
  color: white;
}

.list-group-item {
  background-color: #3f434b;
  border: none;
  color: #ecf0f1;
  font-size: 1rem;
  border-radius: 8px;
  margin-bottom: 10px;
}

.list-group-item:hover {
  background-color: #2c3e50;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.2);
}

.btn {
  border-radius: 5px;
  font-weight: bold;
}

.btn-warning {
  background-color: #f39c12;
  border: none;
}

.btn-warning:hover {
  background-color: #e67e22;
}

.btn-danger {
  background-color: #e74c3c;
  border: none;
}

.btn-danger:hover {
  background-color: #c0392b;
}

.btn-success {
  background-color: #2ecc71;
  border: none;
}

.btn-success:hover {
  background-color: #27ae60;
}

.btn-secondary {
  background-color: #7f8c8d;
  border: none;
}

.btn-secondary:hover {
  background-color: #95a5a6;
}

.form-control {
  background-color: #34495e;
  color: white;
  border: none;
  border-radius: 5px;
}

.form-control::placeholder {
  color: #bdc3c7;
}

.form-control:focus {
  background-color: #2c3e50;
  border-color: #2ecc71;
}
</style>
