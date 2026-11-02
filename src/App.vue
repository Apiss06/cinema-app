<template>
  <div class="cinema-app">
    <!-- Navbar Ala TIX.ID -->
    <header class="navbar">
      <div class="logo">🎬 <span>TIX-CINEMA</span></div>
      <div class="location">📍 Sidoarjo, Darjo Pride</div>
    </header>

    <!-- Banner -->
    <section class="hero">
      <h1>Sedang Tayang di Bioskop</h1>
      <p>Pilih film favoritmu dan amankan kursi terbaikmu sekarang.</p>
    </section>

    <!-- Katalog Film -->
    <main class="movie-container">
      <div v-for="movie in movieList" :key="movie.id" class="movie-card">
        <img :src="movie.poster" :alt="movie.title" class="movie-poster" />
        <div class="movie-info">
          <h3>{{ movie.title }}</h3>
          <p class="genre">{{ movie.genre }}</p>
          <p class="meta">⏱ {{ movie.duration }} | 🏷️ Rp {{ movie.price.toLocaleString() }}</p>
          
          <div class="showtimes">
            <span v-for="(time, index) in movie.showtimes" :key="index" class="badge-time">
              {{ time }}
            </span>
          </div>

          <button @click="openSeatPicker(movie)" class="btn-book">Pilih Jadwal & Kursi</button>
        </div>
      </div>
    </main>

    <!-- Modal Pemilihan Kursi -->
    <SeatPicker 
      v-if="selectedMovie" 
      :movie="selectedMovie" 
      @close="selectedMovie = null" 
      @onSuccess="handleBookingSuccess"
    />

    <!-- Modal E-Tiket QR Code -->
    <TicketModal 
      v-if="activeTicket" 
      :ticket="activeTicket" 
      @close="activeTicket = null"
    />
  </div>
</template>

<script>
import { ref } from 'vue'
import { movies } from './moviesData'
import SeatPicker from './SeatPicker.vue'
import TicketModal from './TicketModal.vue'

export default {
  components: { SeatPicker, TicketModal },
  setup() {
    const movieList = ref(movies)
    const selectedMovie = ref(null)
    const activeTicket = ref(null)

    const openSeatPicker = (movie) => {
      selectedMovie.value = movie
    }

    const handleBookingSuccess = (ticketData) => {
      selectedMovie.value = null // Tutup modal kursi
      activeTicket.value = ticketData // Buka modal tiket QR Code
    }

    return {
      movieList,
      selectedMovie,
      activeTicket,
      openSeatPicker,
      handleBookingSuccess
    }
  }
}
</script>

<style>
/* Style Global */
body { margin: 0; font-family: 'Segoe UI', sans-serif; background-color: #121212; color: #f1f1f1; }
.cinema-app { max-width: 1200px; margin: 0 auto; padding: 20px; }
.navbar { display: flex; justify-content: space-between; align-items: center; background: #1f1f1f; padding: 15px 25px; border-radius: 12px; margin-bottom: 25px; border: 1px solid #333; }
.logo { font-size: 20px; font-weight: bold; color: #0080ff; }
.location { font-size: 14px; color: #aaa; }
.hero { text-align: center; margin-bottom: 30px; }
.hero h1 { font-size: 28px; margin-bottom: 5px; color: #fff; }
.hero p { color: #888; font-size: 14px; }
.movie-container { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 20px; }
.movie-card { background: #1e1e1e; border-radius: 12px; overflow: hidden; display: flex; box-shadow: 0 4px 15px rgba(0,0,0,0.5); border: 1px solid #2c2c2c; }
.movie-poster { width: 120px; object-fit: cover; }
.movie-info { padding: 15px; display: flex; flex-direction: column; justify-content: space-between; width: 100%; }
.movie-info h3 { margin: 0 0 5px 0; font-size: 16px; color: #fff; }
.genre { font-size: 12px; color: #aaa; margin: 0 0 5px 0; }
.meta { font-size: 12px; color: #0080ff; font-weight: bold; margin: 0 0 10px 0; }
.showtimes { display: flex; gap: 6px; margin-bottom: 12px; }
.badge-time { background: #2a2a2a; padding: 4px 8px; font-size: 11px; border-radius: 6px; color: #ddd; border: 1px solid #444; }
.btn-book { background: #0066ff; color: white; border: none; padding: 8px 12px; border-radius: 6px; font-weight: bold; cursor: pointer; }
.btn-book:hover { background: #004bb5; }
</style>