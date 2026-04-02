<template>
  <view class="ticket-buy">
    <view class="movie-cinema-info">
      <view class="movie-info">
        <text class="movie-title">{{ movie.title }}</text>
        <text class="movie-time">{{ selectedTime }} | {{ movie.duration }}</text>
      </view>
      <view class="cinema-info">
        <text class="cinema-name">{{ cinema.name }}</text>
        <text class="cinema-address">{{ cinema.address }}</text>
      </view>
    </view>

    <view class="seat-selection">
      <text class="section-title">选择座位</text>
      <view class="screen">
        <text class="screen-text">屏幕</text>
      </view>
      <scroll-view class="seats-scroll" scroll-x="true">
        <view class="seats-container">
          <view class="row" v-for="(row, rowIndex) in seats" :key="rowIndex">
            <text class="row-number">{{ String.fromCharCode(65 + rowIndex) }}</text>
            <view class="seats">
              <view class="seat"
                    :class="{
                      'occupied': seat.status === 'occupied',
                      'selected': seat.status === 'selected'
                    }"
                    v-for="(seat, seatIndex) in row"
                    :key="seatIndex"
                    @click="selectSeat(rowIndex, seatIndex)">
              </view>
            </view>
          </view>
        </view>
      </scroll-view>
      <view class="seat-legend">
        <view class="legend-item">
          <view class="legend-seat available"></view>
          <text>可选</text>
        </view>
        <view class="legend-item">
          <view class="legend-seat occupied"></view>
          <text>已售</text>
        </view>
        <view class="legend-item">
          <view class="legend-seat selected"></view>
          <text>已选</text>
        </view>
      </view>
    </view>

    <view class="ticket-info">
      <view class="ticket-count">
        <text class="info-label">数量</text>
        <view class="count-control">
          <view class="count-btn" @click="decreaseCount">
            <text>-</text>
          </view>
          <text class="count">{{ ticketCount }}</text>
          <view class="count-btn" @click="increaseCount">
            <text>+</text>
          </view>
        </view>
      </view>
      <view class="ticket-price">
        <text class="info-label">总价</text>
        <text class="price">{{ totalPrice }}元</text>
      </view>
    </view>

    <view class="bottom-bar">
      <view class="selected-seats">
        <text>已选座位: {{ selectedSeats.join(', ') || '无' }}</text>
      </view>
      <view class="buy-btn" @click="confirmPurchase">
        <text>确认购票</text>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      movieId: '',
      cinemaId: '',
      selectedTime: '',
      ticketCount: 1,
      seatPrice: 80,
      seats: [
        [{ status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'occupied' }, { status: 'occupied' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }],
        [{ status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }],
        [{ status: 'occupied' }, { status: 'occupied' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'occupied' }, { status: 'occupied' }, { status: 'available' }, { status: 'available' }],
        [{ status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }],
        [{ status: 'available' }, { status: 'available' }, { status: 'occupied' }, { status: 'occupied' }, { status: 'occupied' }, { status: 'occupied' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }]
      ],
      movie: {
        title: "超级马力欧银河大电影",
        duration: "98分钟"
      },
      cinema: {
        name: "万达影城(北京CBD店)",
        address: "北京市朝阳区建国路87号"
      }
    }
  },
  computed: {
    selectedSeats() {
      const seats = []
      this.seats.forEach((row, rowIndex) => {
        row.forEach((seat, seatIndex) => {
          if (seat.status === 'selected') {
            seats.push(`${String.fromCharCode(65 + rowIndex)}${seatIndex + 1}`)
          }
        })
      })
      return seats
    },
    totalPrice() {
      return this.ticketCount * this.seatPrice
    }
  },
  onLoad(options) {
    this.movieId = options.movieId
    this.cinemaId = options.cinemaId
    this.selectedTime = options.time || '19:30'
    this.seatPrice = parseInt(options.price) || 80
  },
  methods: {
    selectSeat(rowIndex, seatIndex) {
      const seat = this.seats[rowIndex][seatIndex]
      if (seat.status === 'occupied') return
      if (seat.status === 'selected') {
        seat.status = 'available'
      } else {
        seat.status = 'selected'
      }
    },
    decreaseCount() {
      if (this.ticketCount > 1) {
        this.ticketCount--
      }
    },
    increaseCount() {
      if (this.ticketCount < 10) {
        this.ticketCount++
      }
    },
    confirmPurchase() {
      if (this.selectedSeats.length === 0) {
        uni.showToast({
          title: '请选择座位',
          icon: 'none'
        })
        return
      }
      uni.showToast({
        title: '购票成功',
        icon: 'success'
      })
    }
  }
}
</script>

<style scoped>
.ticket-buy {
  background-color: #f5f5f5;
  min-height: 100vh;
  padding-bottom: 140rpx;
}

.movie-cinema-info {
  background-color: #fff;
  padding: 30rpx;
  margin-bottom: 20rpx;
}

.movie-info {
  margin-bottom: 20rpx;
}

.movie-title {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 10rpx;
  display: block;
}

.movie-time {
  font-size: 26rpx;
  color: #666;
  display: block;
}

.cinema-info {
  border-top: 1px solid #eee;
  padding-top: 20rpx;
}

.cinema-name {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 8rpx;
  display: block;
}

.cinema-address {
  font-size: 24rpx;
  color: #999;
  display: block;
}

.seat-selection {
  background-color: #fff;
  padding: 30rpx;
  margin-bottom: 20rpx;
}

.section-title {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 30rpx;
  display: block;
}

.screen {
  width: 100%;
  height: 80rpx;
  background: linear-gradient(to bottom, #f0f0f0, #e0e0e0);
  border-radius: 8rpx;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 40rpx;
}

.screen-text {
  font-size: 26rpx;
  color: #666;
}

.seats-scroll {
  overflow-x: auto;
}

.seats-container {
  display: flex;
  flex-direction: column;
  gap: 20rpx;
  padding: 0 10rpx;
}

.row {
  display: flex;
  align-items: center;
}

.row-number {
  width: 40rpx;
  font-size: 24rpx;
  color: #666;
  margin-right: 20rpx;
  text-align: center;
}

.seats {
  display: flex;
  gap: 12rpx;
}

.seat {
  width: 56rpx;
  height: 56rpx;
  border-radius: 8rpx;
  background-color: #e0e0e0;
  flex-shrink: 0;
}

.seat.occupied {
  background-color: #999;
}

.seat.selected {
  background-color: #ff4d4f;
}

.seat-legend {
  display: flex;
  justify-content: center;
  gap: 60rpx;
  padding-top: 30rpx;
  border-top: 1px solid #eee;
  margin-top: 20rpx;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 10rpx;
  font-size: 24rpx;
  color: #666;
}

.legend-seat {
  width: 40rpx;
  height: 40rpx;
  border-radius: 6rpx;
}

.legend-seat.available {
  background-color: #e0e0e0;
}

.legend-seat.occupied {
  background-color: #999;
}

.legend-seat.selected {
  background-color: #ff4d4f;
}

.ticket-info {
  background-color: #fff;
  padding: 30rpx;
  margin-bottom: 20rpx;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.ticket-count {
  display: flex;
  align-items: center;
  gap: 20rpx;
}

.info-label {
  font-size: 28rpx;
  color: #333;
}

.count-control {
  display: flex;
  align-items: center;
  gap: 30rpx;
}

.count-btn {
  width: 60rpx;
  height: 60rpx;
  border: 2rpx solid #ddd;
  background-color: #fff;
  border-radius: 8rpx;
  display: flex;
  justify-content: center;
  align-items: center;
}

.count-btn text {
  font-size: 32rpx;
  color: #333;
}

.count {
  font-size: 32rpx;
  color: #333;
  min-width: 60rpx;
  text-align: center;
}

.ticket-price {
  display: flex;
  align-items: center;
  gap: 16rpx;
}

.price {
  font-size: 36rpx;
  font-weight: bold;
  color: #ff4d4f;
}

.bottom-bar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: #fff;
  padding: 20rpx 30rpx;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid #eee;
}

.selected-seats {
  flex: 1;
  font-size: 26rpx;
  color: #333;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-right: 20rpx;
}

.buy-btn {
  background-color: #ff4d4f;
  padding: 20rpx 40rpx;
  border-radius: 8rpx;
}

.buy-btn text {
  font-size: 30rpx;
  color: #fff;
  font-weight: bold;
}
</style>
