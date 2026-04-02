<template>
  <view class="index">
    <swiper class="swiper" indicator-dots autoplay interval="3000" duration="500">
      <swiper-item v-for="(item, index) in banners" :key="index">
        <image :src="item.image" class="swiper-image" mode="aspectFill"></image>
      </swiper-item>
    </swiper>

    <view class="quick-entry">
      <view class="entry-item" v-for="(item, index) in quickEntries" :key="index">
        <view class="entry-icon-wrap">
          <image :src="item.icon" class="entry-icon"></image>
        </view>
        <text class="entry-text">{{ item.text }}</text>
      </view>
    </view>

    <view class="hot-movies">
      <view class="section-header">
        <text class="section-title">热门电影</text>
        <navigator url="/pages/movie/list" class="more">更多 ></navigator>
      </view>
      <scroll-view class="movie-list" scroll-x="true" enhanced="true" show-scrollbar="false">
        <view class="movie-item" v-for="(movie, index) in hotMovies" :key="index" @click="goToMovieDetail(movie.id)">
          <image :src="movie.poster" class="movie-poster" mode="aspectFill"></image>
          <text class="movie-title">{{ movie.title }}</text>
          <view class="movie-score" v-if="movie.score">
            <text class="score-text">{{ movie.score }}</text>
          </view>
          <view class="movie-tag" v-else>
            <text class="tag-text">预映</text>
          </view>
        </view>
      </scroll-view>
    </view>

    <view class="hot-activities">
      <view class="section-header">
        <text class="section-title">热门活动</text>
        <navigator url="" class="more">更多 ></navigator>
      </view>
      <view class="activity-list">
        <view class="activity-item" v-for="(activity, index) in activities" :key="index">
          <image :src="activity.image" class="activity-image" mode="aspectFill"></image>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      banners: [
        { image: "https://img.alicdn.com/tfs/TB1XvQhQpXXXXX6XXXXXXXXXXXX-750-400.png" },
        { image: "https://img.alicdn.com/tfs/TB1a5QhQpXXXXX7XXXXXXXXXXXX-750-400.png" },
        { image: "https://img.alicdn.com/tfs/TB1b5QhQpXXXXX8XXXXXXXXXXXX-750-400.png" }
      ],
      quickEntries: [
        { icon: "/static/tabbar/movie.png", text: "热映" },
        { icon: "/static/tabbar/movie.png", text: "即将上映" },
        { icon: "/static/tabbar/cinema.png", text: "影院" },
        { icon: "/static/tabbar/home.png", text: "我的" }
      ],
      hotMovies: [
        { id: 1, title: "超级马力欧银河大电影", score: "9.2", poster: "https://img.alicdn.com/tfs/TB1superMario.jpg" },
        { id: 2, title: "挽救计划", score: "9.3", poster: "https://img.alicdn.com/tfs/TB1rescuePlan.jpg" },
        { id: 3, title: "河狸变身计划", score: "9.4", poster: "https://img.alicdn.com/tfs/TB1beaverPlan.jpg" },
        { id: 4, title: "我的妈耶", score: "", poster: "https://img.alicdn.com/tfs/TB1momYes.jpg" },
        { id: 5, title: "我，许可", score: "", poster: "https://img.alicdn.com/tfs/TB1mePermission.jpg" }
      ],
      activities: [
        { image: "https://img.alicdn.com/tfs/TB1activity1.jpg" },
        { image: "https://img.alicdn.com/tfs/TB1activity2.jpg" }
      ]
    }
  },
  methods: {
    goToMovieDetail(movieId) {
      uni.navigateTo({ url: `/pages/movie/detail?id=${movieId}` })
    }
  }
}
</script>

<style scoped>
.index {
  padding-bottom: 120rpx;
  background-color: #f5f5f5;
  min-height: 100vh;
}

.swiper {
  width: 100%;
  height: 400rpx;
}

.swiper-image {
  width: 100%;
  height: 100%;
}

.quick-entry {
  display: flex;
  justify-content: space-around;
  padding: 40rpx 0;
  background-color: #fff;
  margin-bottom: 20rpx;
}

.entry-item {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.entry-icon-wrap {
  width: 100rpx;
  height: 100rpx;
  background-color: #ff4d4f;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 16rpx;
}

.entry-icon {
  width: 56rpx;
  height: 56rpx;
}

.entry-text {
  font-size: 26rpx;
  color: #333;
}

.hot-movies,
.hot-activities {
  background-color: #fff;
  margin-bottom: 20rpx;
  padding: 30rpx;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30rpx;
}

.section-title {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
}

.more {
  font-size: 24rpx;
  color: #999;
}

.movie-list {
  display: flex;
  white-space: nowrap;
  padding-bottom: 20rpx;
}

.movie-item {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  margin-right: 30rpx;
  width: 200rpx;
}

.movie-poster {
  width: 200rpx;
  height: 280rpx;
  border-radius: 8rpx;
  margin-bottom: 16rpx;
  background-color: #eee;
}

.movie-title {
  font-size: 26rpx;
  color: #333;
  margin-bottom: 8rpx;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  width: 200rpx;
  text-align: center;
}

.movie-score {
  background-color: #ff4d4f;
  padding: 4rpx 12rpx;
  border-radius: 4rpx;
}

.score-text {
  font-size: 22rpx;
  color: #fff;
}

.movie-tag {
  background-color: #ff9900;
  padding: 4rpx 12rpx;
  border-radius: 4rpx;
}

.tag-text {
  font-size: 22rpx;
  color: #fff;
}

.activity-list {
  display: flex;
  flex-direction: column;
  gap: 20rpx;
}

.activity-image {
  width: 100%;
  height: 200rpx;
  border-radius: 8rpx;
  background-color: #eee;
}
</style>
