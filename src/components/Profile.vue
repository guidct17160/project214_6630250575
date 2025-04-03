<template>
  <div class="container mt-5">
    <div class="card profile-card p-5">
      <h2 class="text-left text-light">Profile</h2>
      <div class="">
        <a href="https://www.instagram.com/guidct_/" class="btn btn-secondary mt-3 ms-2" target="_blank">
          <img src="https://cdn-icons-png.flaticon.com/512/2111/2111463.png" alt="Instagram Icon" width="20" height="20" class="me-1">
        </a>
        <a href="https://www.tiktok.com/@ustolemah.3" class="btn btn-secondary mt-3 ms-2" target="_blank">
          <img src="https://cdn-icons-png.flaticon.com/512/3046/3046121.png" alt="TikTok Icon" width="20" height="20" class="me-1">
        </a>
        <a href="https://www.facebook.com/share/1BpFETgccx/" class="btn btn-secondary mt-3 ms-2" target="_blank">
          <img src="https://cdn-icons-png.flaticon.com/512/5968/5968764.png" alt="Facebook Icon" width="20" height="20" class="me-1">
        </a>
      </div>

      <h3 class="personal">Personal Information</h3>

      <div class="d-flex">
        <img :src="profile.image" class="shadow-lg mb-3 profile-img" alt="Profile Picture" />
        <div class="ms-2">
          <div>
            <hr class="ms-5">
            <p class="text-start ms-5"><strong>Name:</strong> {{ profile.name }}</p>
            <p class="text-start ms-5"><strong>Student ID:</strong> {{ profile.id }}</p>
          </div>
          <div>
            <p class="text-start ms-5"><strong>Major:</strong> {{ profile.major }}</p>
            <hr class="ms-5">
            <h3 class="text-center ms-5">Education</h3>
            <hr class="ms-5">
            <p class="text-start ms-5"><strong>School:</strong> {{ profile.school }}</p>
          </div>
        </div>
      </div>

      <!-- ปุ่ม Edit -->
      <button class="btn btn-warning mt-3" @click="editMode=true">Edit</button>

      <!-- ปุ่ม Back -->
      <router-link to="/" class="btn btn-light mt-3 ms-2">
        <img src="https://cdn-icons-png.flaticon.com/32/10294/10294919.png" alt="Back Icon" width="20" height="20" class="me-1">
      </router-link>

      <!-- ปุ่ม Next -->
      <router-link to="/courses" class="btn btn-light mt-3 ms-2">
        <img src="https://cdn-icons-png.flaticon.com/32/10294/10294925.png" alt="Next Icon" width="20" height="20" class="ms-1">
      </router-link>

      <!-- Form สำหรับแก้ไขข้อมูล -->
      <div v-if="editMode" class="mt-4">
        <h3 class="text-center text-light">Edit Profile</h3>
        <form @submit.prevent="saveProfile">
          <input v-model="profile.name" class="form-control mb-2" placeholder="Name" required />
          <input v-model="profile.id" class="form-control mb-2" placeholder="Student ID" required />
          <input v-model="profile.major" class="form-control mb-2" placeholder="Major" required />
          <input v-model="profile.school" class="form-control mb-2" placeholder="Previous School" required />
          <button type="submit" class="btn btn-success mt-3">Save</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      profile: {
        id: "6630250575",
        name: "Dechathon Thongthipyarat",
        major: "Computer Science",
        school: "Maleevid",
        image: "/images/s1.jpg",
      },
      editMode: false,
    };
  },
  methods: {
    async saveProfile() {
      try {
        await fetch("http://localhost:3000/profile", {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(this.profile),
        });

        this.editMode = false;
      } catch (error) {
        console.error("Error saving profile:", error);
      }
    },
  },
  async mounted() {
    try {
      const response = await fetch("http://localhost:3000/profile");
      const data = await response.json();
      if (data) this.profile = data;
    } catch (error) {
      console.error("Error fetching profile:", error);
    }
  },
};
</script>

<style scoped>
/* กรอบของการ์ด */
.profile-card {
  background-color: #2c3e50;
  color: #f9fbfb;
  border-radius: 20px;
  box-shadow: 0 8px 20px rgba(12, 12, 12, 0.3);
  max-width: 1000px;
  margin: auto;
  padding: 60px;
}

/* ฟอนต์ชื่อ */
h2 {
  font-family: 'Link', sans-serif;
  font-size: 3.5rem;
  font-weight: bold;
}

.personal {
  font-family: 'Link', sans-serif;
  font-size: 1.5rem;
  font-weight: bold;
  margin-left: 230px;
}

h3 {
  font-family: 'Link', sans-serif;
  font-size: 1.5rem;
  font-weight: bold;
}

/* รูปโปรไฟล์ */
.profile-img {
  width: 200px;
  height: 300px;
  border: 6px solid #fff;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
  object-fit: cover;
  border-radius: 10px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

/* เพิ่มเอฟเฟกต์ hover เมื่อชี้ที่รูป */
.profile-img:hover {
  transform: scale(1.1); /* ขยายภาพ */
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.6); /* เพิ่มเงาให้ดูนูน */
}

/* ปุ่ม */
.btn {
  font-size: 1.1rem;
  padding: 13px;
  border-radius: 5px;
}

/* ปุ่มกลับ */
.btn-secondary {
  background-color: #2a2d2d;
  border: none;
  transition: all 0.3s ease;
}

.btn-secondary:hover {
  background-color: #95a5a6;
  transform: translateY(-2px);
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

/* ฟอร์ม */
form input {
  font-size: 1rem;
  padding: 10px;
  border-radius: 5px;
  margin-bottom: 15px;
}
</style>
