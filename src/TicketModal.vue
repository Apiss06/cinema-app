<template>
  <div class="modal-overlay">
    <div class="ticket-card">
      <button class="btn-close" @click="$emit('close')">✕</button>
      
      <div class="ticket-header">
        <h3>E-TICKET BIOSKOP</h3>
        <span class="ticket-id">{{ ticket.ticketId }}</span>
      </div>

      <div class="ticket-body">
        <h4>{{ ticket.movieTitle }}</h4>
        <p>🕒 Jam: <strong>{{ ticket.showtime }}</strong></p>
        <p>💺 Kursi: <strong>{{ ticket.seats.join(', ') }}</strong></p>
        <p>💳 Total Bayar: <strong>Rp {{ ticket.totalPrice.toLocaleString() }}</strong></p>
      </div>

      <!-- Container Canvas untuk Gambar QR Code -->
      <div class="qr-container">
        <canvas ref="qrCanvas"></canvas>
        <p class="qr-hint">Tunjukkan QR Code ini ke petugas scanner</p>
      </div>
    </div>
  </div>
</template>

<script>
import { onMounted, ref } from 'vue'
import QRCode from 'qrcode'

export default {
  props: ['ticket'],
  emits: ['close'],
  setup(props) {
    const qrCanvas = ref(null)

    onMounted(() => {
      // Data yang di-encode ke dalam QR Code berupa String JSON
      const qrData = JSON.stringify({
        ticketId: props.ticket.ticketId,
        movie: props.ticket.movieTitle,
        seats: props.ticket.seats,
        showtime: props.ticket.showtime
      })

      QRCode.toCanvas(qrCanvas.value, qrData, { width: 180 }, (error) => {
        if (error) console.error(error)
      })
    })

    return { qrCanvas }
  }
}
</script>

<style scoped>
.modal-overlay {
  position: fixed; top: 0; left: 0; width: 100%; height: 100%;
  background: rgba(0,0,0,0.85); display: flex; justify-content: center;
  align-items: center; z-index: 100;
}
.ticket-card {
  background: #ffffff; color: #111; padding: 25px; border-radius: 16px;
  width: 90%; max-width: 350px; text-align: center; position: relative;
}
.btn-close {
  position: absolute; right: 15px; top: 15px; background: none;
  border: none; color: #666; font-size: 20px; cursor: pointer;
}
.ticket-header { border-bottom: 2px dashed #ccc; padding-bottom: 10px; margin-bottom: 15px; }
.ticket-header h3 { margin: 0; color: #0066ff; }
.ticket-id { font-size: 12px; color: #777; font-weight: bold; }
.ticket-body { text-align: left; margin-bottom: 15px; font-size: 14px; }
.ticket-body h4 { margin: 0 0 10px 0; font-size: 18px; color: #222; }
.qr-container { background: #f5f5f5; padding: 15px; border-radius: 12px; }
.qr-hint { font-size: 11px; color: #666; margin-top: 8px; margin-bottom: 0; }
</style>