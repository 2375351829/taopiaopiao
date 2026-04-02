<template>
  <view class="movie-detail">
    <view class="movie-header">
      <image :src="movie.poster" class="poster-bg"></image>
      <view class="header-overlay"></view>
      <view class="movie-info">
        <image :src="movie.poster" class="movie-poster"></image>
        <view class="info-text">
          <text class="movie-title">{{ movie.title }}</text>
          <view class="movie-score" v-if="movie.score">
            <text class="score-value">{{ movie.score }}</text>
            <text class="score-label">分</text>
          </view>
          <text class="movie-type">{{ movie.type }}</text>
          <text class="movie-meta">{{ movie.duration }} / {{ movie.region }} / {{ movie.language }}</text>
        </view>
      </view>
    </view>

    <view class="movie-desc">
      <text class="section-title">简介</text>
      <text class="desc-text" :class="{ expanded: isExpanded }">{{ movie.description }}</text>
      <text class="expand-btn" @click="isExpanded = !isExpanded">{{ isExpanded ? '收起' : '展开' }}</text>
    </view>

    <view class="cast-section">
      <text class="section-title">演员</text>
      <scroll-view class="cast-list" scroll-x="true" enhanced="true" show-scrollbar="false">
        <view class="cast-item" v-for="(cast, index) in movie.cast" :key="index">
          <view class="cast-avatar"></view>
          <text class="cast-name">{{ cast.name }}</text>
          <text class="cast-role">{{ cast.role }}</text>
        </view>
      </scroll-view>
    </view>

    <view class="showtimes">
      <view class="showtimes-header">
        <text class="section-title">选择场次</text>
        <text class="show-date">{{ currentDate }}</text>
      </view>
      <view class="date-tabs">
        <view class="date-tab" :class="{ active: activeDate === date.date }" v-for="(date, index) in dates" :key="index" @click="selectDate(date.date)">
          <text class="date-day">{{ date.day }}</text>
          <text class="date-week">{{ date.week }}</text>
        </view>
      </view>
      <view class="cinema-list">
        <view class="cinema-item" v-for="(cinema, index) in cinemas" :key="index">
          <view class="cinema-info">
            <text class="cinema-name">{{ cinema.name }}</text>
            <text class="cinema-address">{{ cinema.address }}</text>
            <view class="cinema-tags">
              <text class="tag" v-for="(tag, tagIndex) in cinema.tags" :key="tagIndex">{{ tag }}</text>
            </view>
          </view>
          <view class="time-list">
            <view class="time-item" v-for="(time, timeIndex) in cinema.times" :key="timeIndex" @click="goToBuyTicket(cinema.id, time)">
              <text class="time">{{ time.time }}</text>
              <text class="hall">{{ time.hall }}</text>
              <text class="price">{{ time.price }}元</text>
            </view>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      movieId: '',
      isExpanded: false,
      activeDate: '2026-04-02',
      currentDate: '4月2日 周四',
      dates: [
        { date: '2026-04-02', day: '今天', week: '周四' },
        { date: '2026-04-03', day: '明天', week: '周五' },
        { date: '2026-04-04', day: '后天', week: '周六' },
        { date: '2026-04-05', day: '4月5日', week: '周日' },
        { date: '2026-04-06', day: '4月6日', week: '周一' }
      ],
      movie: {
        id: 1,
        title: "超级马力欧银河大电影",
        score: "9.2",
        type: "喜剧 / 动画 / 冒险 / 动作",
        duration: "98分钟",
        region: "美国",
        language: "英语",
        releaseDate: "2026-04-03",
        description: "超级马力欧银河大电影是亚伦·霍瓦斯、迈克尔·杰勒尼克执导，马修·福格尔担任编剧，克里斯·帕拉特、安雅·泰勒-乔伊、杰克·布莱克、查理·戴、吉甘-迈克尔·凯、布丽·拉尔森等参与配音，照明娱乐公司、任天堂制作的动画电影，该片定为《超级马力欧》的第2部动画电影。",
        poster: "https://img.alicdn.com/tfs/TB1superMario.jpg",
        director: "亚伦·霍瓦斯 / 迈克尔·杰勒尼克",
        cast: [
          { name: "克里斯·帕拉特", role: "马力欧(配音)" },
          { name: "安雅·泰勒-乔伊", role: "桃花公主(配音)" },
          { name: "杰克·布莱克", role: "酷霸王(配音)" },
          { name: "查理·戴", role: "路易吉(配音)" },
          { name: "布丽·拉尔森", role: "碧姬公主(配音)" }
        ]
      },
      cinemas: [
        {
          id: 1,
          name: "万达影城(北京CBD店)",
          address: "北京市朝阳区建国路87号",
          tags: ["杜比厅", "IMAX"],
          times: [
            { time: "10:30", hall: "IMAX厅", price: "79" },
            { time: "13:15", hall: "杜比厅", price: "69" },
            { time: "16:00", hall: "普通厅", price: "49" },
            { time: "19:30", hall: "IMAX厅", price: "89" },
            { time: "22:00", hall: "普通厅", price: "59" }
          ]
        },
        {
          id: 2,
          name: "CGV影城(北京五道口店)",
          address: "北京市海淀区成府路28号",
          tags: ["4DX", "普通厅"],
          times: [
            { time: "10:00", hall: "4DX厅", price: "99" },
            { time: "12:45", hall: "普通厅", price: "45" },
            { time: "15:30", hall: "普通厅", price: "45" },
            { time: "18:15", hall: "4DX厅", price: "99" }
          ]
        },
        {
          id: 3,
          name: "UME影城(北京华星店)",
          address: "北京市海淀区中关村大街15号",
          tags: ["REALD", "普通厅"],
          times: [
            { time: "11:00", hall: "REALD厅", price: "55" },
            { time: "14:00", hall: "普通厅", price: "39" },
            { time: "17:00", hall: "REALD厅", price: "55" },
            { time: "20:00", hall: "普通厅", price: "49" }
          ]
        }
      ]
    }
  },
  onLoad(options) {
    this.movieId = options.id
  },
  methods: {
    selectDate(date) {
      this.activeDate = date
      const dateObj = this.dates.find(d => d.date === date)
      if (dateObj) {
        const day = dateObj.day.replace('今天', '').replace('明天', '').replace('后天', '')
        this.currentDate = `${day}${dateObj.week}`
      }
    },
    goToBuyTicket(cinemaId, time) {
      uni.navigateTo({
        url: `/pages/ticket/buy?movieId=${this.movieId}&cinemaId=${cinemaId}&time=${time.time}&hall=${time.hall}&price=${time.price}`
      })
    }
  }
}
</script>

<style scoped>
.movie-detail {
  background-color: #f5f5f5;
  min-height: 100vh;
  padding-bottom: 40rpx;
}

.movie-header {
  position: relative;
  height: 560rpx;
}

.poster-bg {
  position: absolute;
  width: 100%;
  height: 100%;
  filter: blur(60rpx);
  opacity: 0.3;
}

.header-overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background: linear-gradient(to bottom, rgba(0,0,0,0.6), rgba(0,0,0,0.8));
}

.movie-info {
  position: relative;
  z-index: 1;
  display: flex;
  padding: 60rpx 30rpx;
}

.movie-poster {
  width: 240rpx;
  height: 336rpx;
  border-radius: 12rpx;
  box-shadow: 0 8rpx 24rpx rgba(0, 0, 0, 0.3);
}

.info-text {
  flex: 1;
  margin-left: 30rpx;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  padding-bottom: 20rpx;
}

.movie-title {
  font-size: 40rpx;
  font-weight: bold;
  color: #fff;
  margin-bottom: 20rpx;
}

.movie-score {
  display: flex;
  align-items: baseline;
  margin-bottom: 20rpx;
}

.score-value {
  font-size: 56rpx;
  font-weight: bold;
  color: #ff4d4f;
}

.score-label {
  font-size: 28rpx;
  color: #ff4d4f;
  margin-left: 4rpx;
}

.movie-type {
  font-size: 26rpx;
  color: #ccc;
  margin-bottom: 12rpx;
}

.movie-meta {
  font-size: 24rpx;
  color: #999;
}

.movie-desc {
  background-color: #fff;
  padding: 30rpx;
  margin-bottom: 20rpx;
  position: relative;
}

.section-title {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 20rpx;
  display: block;
}

.desc-text {
  font-size: 28rpx;
  color: #666;
  line-height: 1.8;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.desc-text.expanded {
  display: block;
}

.expand-btn {
  font-size: 26rpx;
  color: #ff4d4f;
  margin-top: 16rpx;
}

.cast-section {
  background-color: #fff;
  padding: 30rpx;
  margin-bottom: 20rpx;
}

.cast-list {
  display: flex;
  white-space: nowrap;
  gap: 30rpx;
}

.cast-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 140rpx;
  flex-shrink: 0;
}

.cast-avatar {
  width: 110rpx;
  height: 110rpx;
  border-radius: 50%;
  background: linear-gradient(135deg, #ff4d4f, #ff9900);
  margin-bottom: 12rpx;
}

.cast-name {
  font-size: 24rpx;
  color: #333;
  text-align: center;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  width: 100%;
}

.cast-role {
  font-size: 20rpx;
  color: #999;
  text-align: center;
}

.showtimes {
  background-color: #fff;
  padding: 30rpx;
}

.showtimes-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30rpx;
}

.show-date {
  font-size: 26rpx;
  color: #ff4d4f;
}

.date-tabs {
  display: flex;
  overflow-x: auto;
  gap: 20rpx;
  margin-bottom: 40rpx;
  padding-bottom: 10rpx;
}

.date-tab {
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20rpx 24rpx;
  border-radius: 12rpx;
  background-color: #f5f5f5;
  min-width: 110rpx;
}

.date-tab.active {
  background-color: #ff4d4f;
}

.date-day {
  font-size: 28rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 4rpx;
}

.date-week {
  font-size: 22rpx;
  color: #999;
}

.date-tab.active .date-day,
.date-tab.active .date-week {
  color: #fff;
}

.cinema-list {
  display: flex;
  flex-direction: column;
  gap: 30rpx;
}

.cinema-item {
  border-top: 1px solid #eee;
  padding-top: 30rpx;
}

.cinema-info {
  margin-bottom: 24rpx;
}

.cinema-name {
  font-size: 30rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 8rpx;
  display: block;
}

.cinema-address {
  font-size: 24rpx;
  color: #999;
  display: block;
  margin-bottom: 12rpx;
}

.cinema-tags {
  display: flex;
  gap: 12rpx;
}

.tag {
  font-size: 20rpx;
  color: #ff9900;
  background-color: #fff5e6;
  padding: 4rpx 12rpx;
  border-radius: 4rpx;
}

.time-list {
  display: flex;
  flex-wrap: wrap;
  gap: 20rpx;
}

.time-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20rpx 28rpx;
  border: 1px solid #ddd;
  border-radius: 8rpx;
  min-width: 150rpx;
}

.time {
  font-size: 30rpx;
  font-weight: bold;
  color: #333;
  margin-bottom: 4rpx;
}

.hall {
  font-size: 20rpx;
  color: #999;
  margin-bottom: 8rpx;
}

.price {
  font-size: 28rpx;
  color: #ff4d4f;
  font-weight: bold;
}
</style>
