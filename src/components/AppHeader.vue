<template>
  <header class="app-header">
    <div class="logo" @click="goHome"> Scooter 🛵  Sharing</div>
  </header>
</template>

<script>
import { useRouter } from 'vue-router'
export default {
  setup() {
    const router = useRouter()
    const goHome = () => router.push('/')
    return { goHome }
  }
}
</script>

<style scoped>
.app-header {
  position: sticky;
  top:0;
  display:flex;
  align-items:center;
  padding:30px 60px;
  background:#697798;
  color:white;
  z-index:10;
}

.logo { 
  cursor: pointer; 
  font-weight:400; 
  font-size:1.6em; 
}

.logo:hover { 
  text-decoration: underline  #f3cda7;;
}
</style>
