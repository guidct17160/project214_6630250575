<template>
  <div class="home-container">
    <div class="content-box text-center p-4" :class="{'fade-in': isVisible}">
      <h1 class="portfolio-title">PORTFOLIO</h1>
      <img src="/images/s1.jpg" class="profile-img mt-3" alt="Portfolio Image" />
      <div class="Conduit">
        <router-link to="/profile" class="btn btn-primary btn-custom me-2 mt-4">Go to Profile</router-link>
        <router-link to="/courses" class="btn btn-success btn-custom me-2 mt-4">Go to Courses</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      isVisible: false
    };
  },
  mounted() {
    setTimeout(() => {
      this.isVisible = true; // Make the content fade in after the page loads
    }, 100); // You can adjust the delay as needed
  }
};
</script>

<style scoped>
/* พื้นหลังสีเทาหรู ๆ */
.home-container {
  background-color: #000000;
  height: 90vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* การเคลื่อนไหวให้คอนเทนต์โหลดแบบนุ่มๆ */
.content-box {
  background: #ffffff;
  border: 10px solid rgb(75, 72, 72);
  border-radius: 50px;
  padding: 10px;
  width: 500px;
  height: 550px;
  box-shadow: 8px 8px 20px rgba(0, 0, 0, 0.3);
  opacity: 0; /* เริ่มต้นที่โปร่งใสน้อย */
  transform: translateY(30px); /* เริ่มต้นที่เคลื่อนที่ลงมา */
  transition: opacity 0.6s ease-out, transform 0.6s ease-out; /* ทำให้มีการเปลี่ยนแปลง */
}

.content-box.fade-in {
  opacity: 1; /* เปลี่ยนเป็นโปร่งใสเต็มที่ */
  transform: translateY(0); /* ทำให้กลับมาอยู่ที่ตำแหน่งเดิม */
}

/* รูปภาพโปรไฟล์เป็นวงกลม + กรอบดำ */
.profile-img {
  width: 300px;
  height: 300px;
  border-radius: 10%;
  border: 3px solid black;
  object-fit: cover;
  transition: transform 0.3s ease, box-shadow 0.3s ease; /* เพิ่ม transition สำหรับการเปลี่ยนแปลง */
}

/* เมื่อชี้เมาส์บนภาพ */
.profile-img:hover {
  transform: scale(1.1); /* ทำให้ภาพขยายขึ้นเล็กน้อย */
  box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.3); /* เพิ่มเงาเพื่อให้ภาพดูนูนขึ้น */
}

/* ฟอนต์ PORTFOLIO ดูหรู + ขยายใหญ่ */
.portfolio-title {
  font-family: 'Playfair Display', serif;
  font-size: 2.8rem;
  font-weight: bold;
  color: black;
  text-transform: uppercase;
}

/* ปุ่มให้ดูสวยขึ้น */
.btn-custom {
  font-size: 1.1rem;
  padding: 10px 20px;
  transition: all 0.3s ease-in-out;
}

/* เอฟเฟกต์ hover ให้ปุ่มดูนูนขึ้น */
.btn-custom:hover {
  transform: translateY(-2px);
  box-shadow: 2px 4px 10px rgba(0, 0, 0, 0.2);
}
</style>
