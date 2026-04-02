<template>
  <view class="movie-detail">
    <!-- 电影信息 -->
    <view class="movie-info">
      <image :src="movie.poster" class="movie-poster"></image>
      <view class="info-content">
        <text class="movie-title">{{ movie.title }}</text>
        <text class="movie-score">{{ movie.score }}</text>
        <text class="movie-desc">{{ movie.desc }}</text>
        <view class="movie-meta">
          <text class="meta-item">{{ movie.duration }}</text>
          <text class="meta-item">{{ movie.releaseDate }}</text>
          <text class="meta-item">{{ movie.region }}</text>
        </view>
        <view class="movie-tags">
          <text class="tag" v-for="(tag, index) in movie.tags" :key="index">{{ tag }}</text>
        </view>
      </view>
    </view>

    <!-- 演员信息 -->
    <view class="cast-info">
      <text class="section-title">演员</text>
      <view class="cast-list">
        <view class="cast-item" v-for="(cast, index) in movie.cast" :key="index">
          <image :src="cast.avatar" class="cast-avatar"></image>
          <text class="cast-name">{{ cast.name }}</text>
          <text class="cast-role">{{ cast.role }}</text>
        </view>
      </view>
    </view>

    <!-- 场次选择 -->
    <view class="showtimes">
      <text class="section-title">选择场次</text>
      <view class="date-tabs">
        <view class="date-tab" :class="{ active: activeDate === date.date }" v-for="(date, index) in dates" :key="index" @click="activeDate = date.date">
          <text class="date-day">{{ date.day }}</text>
          <text class="date-week">{{ date.week }}</text>
        </view>
      </view>
      <view class="cinema-list">
        <view class="cinema-item" v-for="(cinema, index) in cinemas" :key="index">
          <view class="cinema-info">
            <text class="cinema-name">{{ cinema.name }}</text>
            <text class="cinema-address">{{ cinema.address }}</text>
          </view>
          <view class="time-list">
            <view class="time-item" @click="goToBuyTicket(cinema.id, time)" v-for="(time, timeIndex) in cinema.times" :key="timeIndex">
              <text class="time">{{ time.time }}</text>
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
      activeDate: '2026-04-02',
      dates: [
        { date: '2026-04-02', day: '今天', week: '周四' },
        { date: '2026-04-03', day: '明天', week: '周五' },
        { date: '2026-04-04', day: '后天', week: '周六' },
        { date: '2026-04-05', day: '4月5日', week: '周日' },
        { date: '2026-04-06', day: '4月6日', week: '周一' }
      ],
      movie: {
        id: 1,
        title: "阿凡达3",
        score: "9.2",
        desc: "詹姆斯·卡梅隆执导的科幻巨制，讲述人类与纳美人的冲突与和解。故事发生在潘多拉星球，人类为了获取资源再次来到这里，与纳美人展开了新一轮的冲突。杰克·萨利和奈蒂莉必须带领纳美人保卫家园，同时揭开潘多拉星球的更多秘密。",
        duration: "180分钟",
        releaseDate: "2026-03-15",
        region: "美国",
        tags: ["科幻", "冒险", "3D"],
        poster: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=Avatar%203%20movie%20poster%202026&image_size=portrait_4_3",
        cast: [
          { name: "萨姆·沃辛顿", role: "杰克·萨利", avatar: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=actor%20Sam%20Worthington%20portrait&image_size=square" },
          { name: "佐伊·索尔达娜", role: "奈蒂莉", avatar: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=actress%20Zoe%20Saldana%20portrait&image_size=square" },
          { name: "西格妮·韦弗", role: "格蕾丝·奥古斯汀", avatar: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=actress%20Sigourney%20Weaver%20portrait&image_size=square" }
        ]
      },
      cinemas: [
        {
          id: 1,
          name: "万达影城(北京CBD店)",
          address: "北京市朝阳区建国路87号",
          times: [
            { time: "10:00", price: "60" },
            { time: "13:30", price: "80" },
            { time: "16:00", price: "80" },
            { time: "19:30", price: "100" },
            { time: "22:00", price: "80" }
          ]
        },
        {
          id: 2,
          name: "CGV影城(北京五道口店)",
          address: "北京市海淀区成府路28号",
          times: [
            { time: "10:30", price: "55" },
            { time: "14:00", price: "75" },
            { time: "17:30", price: "75" },
            { time: "20:00", price: "95" }
          ]
        },
        {
          id: 3,
          name: "UME影城(北京华星店)",
          address: "北京市海淀区中关村大街15号",
          times: [
            { time: "11:00", price: "58" },
            { time: "14:30", price: "78" },
            { time: "18:00", price: "78" },
            { time: "21:30", price: "98" }
          ]
        }
      ]
    }
  },
  onLoad(options) {
    this.movieId = options.id
    // 实际项目中这里会根据movieId请求电影详情数据
  },
  methods: {
    goToBuyTicket(cinemaId, time) {
      uni.navigateTo({
        url: `/pages/ticket/buy?movieId=${this.movieId}&cinemaId=${cinemaId}&time=${time.time}`
      })
    }
  }
}
</script>

<style scoped>
.movie-detail {
  background-color: #f5f5f5;
  min-height: 100vh;
}

.movie-info {
  display: flex;
  background-color: #fff;
  padding: 20px;
  margin-bottom: 10px;
}

.movie-poster {
  width: 120px;
  height: 168px;
  border-radius: 4px;
  margin-right: 20px;
}

.info-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.movie-title {
  font-size: 20px;
  font-weight: bold;
  color: #333;
  margin-bottom: 8px;
}

.movie-score {
  font-size: 16px;
  color: #ff4d4f;
  margin-bottom: 12px;
}

.movie-desc {
  font-size: 14px;
  color: #666;
  line-height: 1.5;
  margin-bottom: 12px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
}

.movie-meta {
  display: flex;
  gap: 15px;
  margin-bottom: 10px;
}

.meta-item {
  font-size: 12px;
  color: #999;
}

.movie-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.tag {
  font-size: 12px;
  color: #666;
  background-color: #f0f0f0;
  padding: 2px 8px;
  border-radius: 4px;
}

.cast-info {
  background-color: #fff;
  padding: 15px;
  margin-bottom: 10px;
}

.section-title {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin-bottom: 15px;
}

.cast-list {
  display: flex;
  overflow-x: auto;
  gap: 15px;
  padding-bottom: 10px;
}

.cast-item {
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 80px;
}

.cast-avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  margin-bottom: 8px;
}

.cast-name {
  font-size: 12px;
  color: #333;
  margin-bottom: 4px;
  text-align: center;
}

.cast-role {
  font-size: 10px;
  color: #999;
  text-align: center;
}

.showtimes {
  background-color: #fff;
  padding: 15px;
}

.date-tabs {
  display: flex;
  overflow-x: auto;
  gap: 10px;
  margin-bottom: 20px;
  padding-bottom: 10px;
}

.date-tab {
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px;
  border-radius: 4px;
  background-color: #f5f5f5;
  min-width: 60px;
}

.date-tab.active {
  background-color: #ff4d4f;
  color: #fff;
}

.date-day {
  font-size: 14px;
  font-weight: bold;
  margin-bottom: 4px;
}

.date-week {
  font-size: 12px;
}

.cinema-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.cinema-item {
  border-top: 1px solid #eee;
  padding-top: 15px;
}

.cinema-info {
  margin-bottom: 10px;
}

.cinema-name {
  font-size: 14px;
  font-weight: bold;
  color: #333;
  margin-bottom: 4px;
  display: block;
}

.cinema-address {
  font-size: 12px;
  color: #999;
  display: block;
}

.time-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.time-item {
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 70px;
}

.time {
  font-size: 14px;
  color: #333;
  margin-bottom: 4px;
}

.price {
  font-size: 12px;
  color: #ff4d4f;
}
</style>