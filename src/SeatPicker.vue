<template>
  <div class="modal-overlay">
    <div class="modal-content">
      <button class="btn-close" @click="$emit('close')">✕</button>
      
      <h2>Pilih Kursi - {{ movie.title }}</h2>
      
      <!-- Pilihan Jam Tayang -->
      <div class="time-selection">
        <p>Pilih Jam Tayang:</p>
        <div class="time-options">
          <button 
            v-for="time in movie.showtimes" 
            :key="time"
            :class="['btn-time', { active: selectedTime === time }]"
            @click="selectedTime = time"
          >
            {{ time }}
          </button>
        </div>
      </div>

      <!-- Visual Layar Bioskop -->
      <div class="screen-container">
        <div class="screen">LAYAR BIOSKOP</div>
      </div>

      <!-- Grid Denah Kursi -->
      <div class="seat-grid">
        <div v-for="seat in seats" :key="seat.id" class="seat-item">
          <button 
            :class="[
              'seat-btn', 
              seat.status, 
              { selected: selectedSeats.includes(seat.id) }
            ]"
            :disabled="seat.status === 'occupied'"
            @click="toggleSeat(seat.id)"
          >
            {{ seat.id }}
          </button>
        </div>
      </div>

      <!-- Keterangan Warna (Legend) -->
      <div class="legend">
        <span><i class="box available"></i> Tersedia</span>
        <span><i class="box selected"></i> Terpilih</span>
        <span><i class="box occupied"></i> Terjual</span>
      </div>

      <!-- Ringkasan & Tombol Bayar -->
      <div class="checkout-summary">
        <div>
          <p>Kursi Dipilih: <strong>{{ selectedSeats.join(', ') || '-' }}</strong></p>
          <p class="total-price">Total: Rp {{ totalPrice.toLocaleString() }}</p>
        </div>
        <button 
          class="btn-checkout" 
          :disabled="selectedSeats.length === 0 || !selectedTime"
          @click="confirmBooking"
        >
          Konfirmasi & Cetak Tiket
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'

export default {
  props: ['movie'],
  emits: ['close', 'onSuccess'],
  setup(props, { emit }) {
    const selectedTime = ref(props.movie.showtimes[0])
    const selectedSeats = ref([])

    // Data 15 Kursi (A1-A5, B1-B5, C1-C5) dengan simulasi beberapa kursi terisi
    const seats = ref([
      { id: 'A1', status: 'available' }, { id: 'A2', status: 'available' }, { id: 'A3', status: 'occupied' }, { id: 'A4', status: 'available' }, { id: 'A5', status: 'available' },
      { id: 'B1', status: 'available' }, { id: 'B2', status: 'available' }, { id: 'B3', status: 'available' }, { id: 'B4', status: 'occupied' }, { id: 'B5', status: 'available' },
      { id: 'C1', status: 'available' }, { id: 'C2', status: 'available' }, { id: 'C3', status: 'available' }, { id: 'C4', status: 'available' }, { id: 'C5', status: 'available' }
    ])

    const toggleSeat = (seatId) => {
      const index = selectedSeats.value.indexOf(seatId)
      if (index === -1) {
        selectedSeats.value.push(seatId)
      } else {
        selectedSeats.value.splice(index, 1)
      }
    }

    const totalPrice = computed(() => {
      return selectedSeats.value.length * props.movie.price
    })

    const confirmBooking = () => {
      const ticketData = {
        ticketId: 'TIX-' + Math.floor(100000 + Math.random() * 900000),
        movieTitle: props.movie.title,
        showtime: selectedTime.value,
        seats: selectedSeats.value,
        totalPrice: totalPrice.value,
        bookingTime: new Date().toLocaleString()
      }
      emit('onSuccess', ticketData)
    }

    return {
      selectedTime,
      seats,
      selectedSeats,
      toggleSeat,
      totalPrice,
      confirmBooking
    }
  }
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.85);
  display: flex; justify-content: center; align-items: center;
  z-index: 100;
}
.modal-content {
  background: #1e1e1e; padding: 25px; border-radius: 16px;
  width: 90%; max-width: 500px; color: #fff; position: relative;
  border: 1px solid #333;
}
.btn-close {
  position: absolute; right: 15px; top: 15px; background: none;
  border: none; color: #aaa; font-size: 20px; cursor: pointer;
}
.time-selection { margin-bottom: 20px; }
.time-options { display: flex; gap: 10px; margin-top: 8px; }
.btn-time {
  background: #2a2a2a; border: 1px solid #444; color: #fff;
  padding: 6px 12px; border-radius: 6px; cursor: pointer;
}
.btn-time.active { background: #0066ff; border-color: #0066ff; }
.screen-container { margin: 20px 0; text-align: center; }
.screen {
  background: #0080ff; color: #fff; padding: 4px; font-size: 12px;
  font-weight: bold; border-radius: 4px; letter-spacing: 2px;
  box-shadow: 0 0 15px rgba(0, 128, 255, 0.4);
}
.seat-grid {
  display: grid; grid-template-columns: repeat(5, 1fr); gap: 10px;
  margin-bottom: 20px;
}
.seat-btn {
  padding: 10px; border-radius: 8px; border: none; font-weight: bold;
  cursor: pointer; background: #333; color: #fff;
}
.seat-btn.occupied { background: #444; color: #777; cursor: not-allowed; }
.seat-btn.selected { background: #00ff88; color: #000; }
.legend { display: flex; justify-content: space-around; font-size: 12px; margin-bottom: 20px; }
.legend span { display: flex; align-items: center; gap: 5px; }
.box { width: 12px; height: 12px; border-radius: 3px; display: inline-block; }
.box.available { background: #333; }
.box.selected { background: #00ff88; }
.box.occupied { background: #444; }
.checkout-summary {
  display: flex; justify-content: space-between; align-items: center;
  border-top: 1px solid #333; padding-top: 15px;
}
.total-price { color: #00ff88; font-weight: bold; margin-top: 4px; }
.btn-checkout {
  background: #0066ff; color: #fff; border: none; padding: 10px 16px;
  border-radius: 8px; font-weight: bold; cursor: pointer;
}
.btn-checkout:disabled { background: #444; cursor: not-allowed; }
</style>