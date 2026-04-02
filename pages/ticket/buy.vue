<template>
  <view class="ticket-buy">
    <!-- 电影和影院信息 -->
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

    <!-- 座位选择 -->
    <view class="seat-selection">
      <text class="section-title">选择座位</text>
      <view class="screen">
        <text class="screen-text">屏幕</text>
      </view>
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
              <text class="seat-number">{{ seatIndex + 1 }}</text>
            </view>
          </view>
        </view>
      </view>
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

    <!-- 购票数量和价格 -->
    <view class="ticket-info">
      <view class="ticket-count">
        <text>数量</text>
        <view class="count-control">
          <button class="count-btn" @click="decreaseCount" :disabled="ticketCount <= 1">-</button>
          <text class="count">{{ ticketCount }}</text>
          <button class="count-btn" @click="increaseCount" :disabled="ticketCount >= 10">+</button>
        </view>
      </view>
      <view class="ticket-price">
        <text>总价</text>
        <text class="price">{{ totalPrice }}元</text>
      </view>
    </view>

    <!-- 底部按钮 -->
    <view class="bottom-bar">
      <view class="selected-seats">
        <text>已选座位: {{ selectedSeats.join(', ') }}</text>
      </view>
      <button class="buy-btn" @click="confirmPurchase">确认购票</button>
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
        [{ status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'occupied' }, { status: 'occupied' }, { status: 'available' }, { status: 'available' }, { status: 'available' }],
        [{ status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }],
        [{ status: 'occupied' }, { status: 'occupied' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'occupied' }, { status: 'occupied' }],
        [{ status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }, { status: 'available' }],
        [{ status: 'available' }, { status: 'available' }, { status: 'occupied' }, { status: 'occupied' }, { status: 'occupied' }, { status: 'occupied' }, { status: 'available' }, { status: 'available' }]
      ],
      movie: {
        title: "阿凡达3",
        duration: "180分钟"
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
    this.selectedTime = options.time
    // 实际项目中这里会根据movieId和cinemaId请求相关数据
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
      // 实际项目中这里会调用支付接口
    }
  }
}
</script>

<style scoped>
.ticket-buy {
  background-color: #f5f5f5;
  min-height: 100vh;
  padding-bottom: 60px;
}

.movie-cinema-info {
  background-color: #fff;
  padding: 15px;
  margin-bottom: 10px;
}

.movie-info {
  margin-bottom: 10px;
}

.movie-title {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin-bottom: 5px;
  display: block;
}

.movie-time {
  font-size: 14px;
  color: #666;
  display: block;
}

.cinema-info {
  border-top: 1px solid #eee;
  padding-top: 10px;
}

.cinema-name {
  font-size: 14px;
  font-weight: bold;
  color: #333;
  margin-bottom: 5px;
  display: block;
}

.cinema-address {
  font-size: 12px;
  color: #999;
  display: block;
}

.seat-selection {
  background-color: #fff;
  padding: 15px;
  margin-bottom: 10px;
}

.section-title {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin-bottom: 15px;
  display: block;
}

.screen {
  width: 100%;
  height: 40px;
  background-color: #f0f0f0;
  border-radius: 4px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 30px;
}

.screen-text {
  font-size: 14px;
  color: #666;
}

.seats-container {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-bottom: 20px;
}

.row {
  display: flex;
  align-items: center;
}

.row-number {
  width: 20px;
  font-size: 12px;
  color: #666;
  margin-right: 10px;
}

.seats {
  display: flex;
  gap: 10px;
  flex: 1;
}

.seat {
  width: 30px;
  height: 30px;
  border-radius: 4px;
  background-color: #e0e0e0;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 12px;
  color: #666;
}

.seat.occupied {
  background-color: #999;
  color: #fff;
}

.seat.selected {
  background-color: #ff4d4f;
  color: #fff;
}

.seat-legend {
  display: flex;
  justify-content: space-around;
  padding-top: 15px;
  border-top: 1px solid #eee;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 12px;
  color: #666;
}

.legend-seat {
  width: 20px;
  height: 20px;
  border-radius: 2px;
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
  padding: 15px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.ticket-count {
  display: flex;
  align-items: center;
  gap: 10px;
}

.count-control {
  display: flex;
  align-items: center;
  gap: 15px;
}

.count-btn {
  width: 30px;
  height: 30px;
  border: 1px solid #ddd;
  background-color: #fff;
  border-radius: 4px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 16px;
}

.count {
  font-size: 16px;
  color: #333;
  min-width: 30px;
  text-align: center;
}

.ticket-price {
  display: flex;
  align-items: center;
  gap: 10px;
}

.price {
  font-size: 18px;
  font-weight: bold;
  color: #ff4d4f;
}

.bottom-bar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: #fff;
  padding: 10px 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid #eee;
}

.selected-seats {
  flex: 1;
  font-size: 14px;
  color: #333;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.buy-btn {
  background-color: #ff4d4f;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  font-size: 14px;
  font-weight: bold;
}
</style>