<template>
  <section class="account-layout">
    <div class="main-content">
      <Sidebar class="mobile-hidden" />
      <div class="account-content">
        <router-view />
        <BottomBar class="desktop-hidden" />
      </div>
    </div>
    <Footer />
  </section>
</template>

<script setup>
import Sidebar from "@/components/Sidebar.vue";
import BottomBar from "@/components/BottomBar.vue";
import Footer from "@/components/Footer.vue";
</script>

<style scoped>
.account-layout {
  display: grid;
  grid-template-rows: 1fr auto;
  min-height: 100vh;
  padding-top: 100px; /* offset for fixed navbar height */
}

.main-content {
  display: flex;
  height: 100%;
  min-height: 0; /* allow flex children to shrink */
}

.account-content {
  flex: 1;
  padding: 24px;
  background-color: #fff;
  overflow-x: hidden;
  overflow-y: auto;
}

.Sidebar {
  flex: 0 0 250px; /* fixed width sidebar */
}

/* ⬇️ Sembunyikan BottomBar di desktop */
.desktop-hidden {
  display: none;
}

@media (max-width: 768px) {
  .main-content {
    flex-direction: column;
  }

  .account-content {
    padding: 16px;
    padding-bottom: 60px; /* agar tidak ketutup BottomBar */
  }

  .desktop-hidden {
    display: flex;
  }
}
</style>
