<template>
  <view class="movie-list">
    <view class="tab-bar">
      <view class="tab-item" :class="{ active: activeTab === 'hot' }" @click="activeTab = 'hot'">
        <text>热映</text>
      </view>
      <view class="tab-item" :class="{ active: activeTab === 'coming' }" @click="activeTab = 'coming'">
        <text>即将上映</text>
      </view>
    </view>

    <view class="movies-container">
      <view v-if="activeTab === 'hot'" class="movies">
        <view class="movie-card" v-for="(movie, index) in hotMovies" :key="index" @click="goToMovieDetail(movie.id)">
          <image :src="movie.poster" class="movie-poster" mode="aspectFill"></image>
          <view class="movie-info">
            <view class="movie-header">
              <text class="movie-title">{{ movie.title }}</text>
              <view class="movie-score" v-if="movie.score">
                <text class="score-value">{{ movie.score }}</text>
              </view>
            </view>
            <text class="movie-type">{{ movie.type }}</text>
            <text class="movie-actors">{{ movie.actors }}</text>
            <view class="movie-footer">
              <text class="movie-count" v-if="movie.wantCount">{{ movie.wantCount }}人想看</text>
              <view class="buy-btn" v-if="movie.isShow">
                <text>选座购票</text>
              </view>
              <view class="presell-btn" v-else>
                <text>预映</text>
              </view>
            </view>
          </view>
        </view>
      </view>

      <view v-else class="movies">
        <view class="movie-card" v-for="(movie, index) in comingMovies" :key="index" @click="goToMovieDetail(movie.id)">
          <image :src="movie.poster" class="movie-poster" mode="aspectFill"></image>
          <view class="movie-info">
            <view class="movie-header">
              <text class="movie-title">{{ movie.title }}</text>
            </view>
            <text class="movie-type">{{ movie.type }}</text>
            <text class="movie-actors">{{ movie.actors }}</text>
            <view class="movie-footer">
              <text class="movie-date">{{ movie.releaseDate }}</text>
              <view class="presell-btn">
                <text>预映</text>
              </view>
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
      activeTab: 'hot',
      hotMovies: [
        { id: 1, title: "超级马力欧银河大电影", score: "9.2", type: "喜剧 / 动画 / 冒险 / 动作", actors: "克里斯·帕拉特 / 安雅·泰勒-乔伊 / 杰克·布莱克", releaseDate: "2026-04-03", isShow: true, wantCount: "4.9万", poster: "https://img.alicdn.com/tfs/TB1superMario.jpg" },
        { id: 2, title: "挽救计划", score: "9.3", type: "科幻 / 冒险 / 动作", actors: "瑞恩·高斯林 / 桑德拉·惠勒 / 詹姆斯·奥尔蒂斯", releaseDate: "2026-03-20", isShow: true, wantCount: "", poster: "https://img.alicdn.com/tfs/TB1rescuePlan.jpg" },
        { id: 3, title: "河狸变身计划", score: "9.4", type: "冒险 / 喜剧 / 动画", actors: "佩珀·柯达 / 乔恩·哈姆 / 鲍比·莫尼汉", releaseDate: "2026-03-28", isShow: true, wantCount: "", poster: "https://img.alicdn.com/tfs/TB1beaverPlan.jpg" },
        { id: 4, title: "我的妈耶", score: "", type: "家庭 / 剧情 / 喜剧", actors: "马思纯 / 白客 / 黄明昊 / 锤娜丽莎", releaseDate: "2026-04-03", isShow: false, wantCount: "38万", poster: "https://img.alicdn.com/tfs/TB1momYes.jpg" },
        { id: 5, title: "我，许可", score: "", type: "剧情 / 喜剧", actors: "文淇 / 秦海璐 / 白客", releaseDate: "2026-04-03", isShow: false, wantCount: "45万", poster: "https://img.alicdn.com/tfs/TB1mePermission.jpg" },
        { id: 6, title: "北回归线以北", score: "", type: "剧情 / 冒险", actors: "任达华 / 王姬 / 赵汉唐 / 江一燕", releaseDate: "2026-04-03", isShow: false, wantCount: "", poster: "https://img.alicdn.com/tfs/TB1northRegression.jpg" }
      ],
      comingMovies: [
        { id: 7, title: "天才游戏", type: "悬疑 / 犯罪", actors: "彭昱畅 / 丁禹兮", releaseDate: "2026-04-04", wantCount: "3.2万", poster: "https://img.alicdn.com/tfs/TB1geniusGame.jpg" },
        { id: 8, title: "角头：大桥头", type: "动作 / 犯罪", actors: "施名帅 / 郑人硕", releaseDate: "2026-04-10", wantCount: "", poster: "https://img.alicdn.com/tfs/TB1gangster.jpg" },
        { id: 9, title: "穿普拉达的女王2", type: "剧情 / 喜剧", actors: "梅丽尔·斯特里普 / 安妮·海瑟薇 / 艾米莉·布朗特", releaseDate: "2026-04-30", wantCount: "4.1万", poster: "https://img.alicdn.com/tfs/TB1devilWearsPrada2.jpg" },
        { id: 10, title: "迈克尔·杰克逊：巨星之路", type: "剧情 / 音乐 / 传记", actors: "贾法尔·杰克逊 / 科尔曼·多明戈 / 尼娅·朗", releaseDate: "2026-04-24", wantCount: "2.5万", poster: "https://img.alicdn.com/tfs/TB1michaelJackson.jpg" },
        { id: 11, title: "小黄人与大怪兽", type: "喜剧 / 科幻 / 动画 / 冒险", actors: "皮埃尔·柯芬", releaseDate: "2026-05-01", wantCount: "1.7万", poster: "https://img.alicdn.com/tfs/TB1minions.jpg" }
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
.movie-list {
  background-color: #f5f5f5;
  min-height: 100vh;
}

.tab-bar {
  display: flex;
  background-color: #fff;
  border-bottom: 1px solid #eee;
  position: sticky;
  top: 0;
  z-index: 100;
}

.tab-item {
  flex: 1;
  text-align: center;
  padding: 30rpx 0;
  position: relative;
  color: #666;
  font-size: 30rpx;
}

.tab-item.active {
  color: #ff4d4f;
  font-weight: bold;
}

.tab-item.active::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 25%;
  width: 50%;
  height: 4rpx;
  background-color: #ff4d4f;
}

.movies-container {
  padding: 20rpx;
}

.movies {
  display: flex;
  flex-direction: column;
  gap: 24rpx;
}

.movie-card {
  display: flex;
  background-color: #fff;
  border-radius: 16rpx;
  padding: 30rpx;
}

.movie-poster {
  width: 180rpx;
  height: 252rpx;
  border-radius: 8rpx;
  margin-right: 30rpx;
  background-color: #eee;
  flex-shrink: 0;
}

.movie-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  min-width: 0;
}

.movie-header {
  display: flex;
  align-items: center;
  margin-bottom: 12rpx;
}

.movie-title {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.movie-score {
  background-color: #ff4d4f;
  padding: 4rpx 12rpx;
  border-radius: 4rpx;
  margin-left: 16rpx;
  flex-shrink: 0;
}

.score-value {
  font-size: 24rpx;
  color: #fff;
  font-weight: bold;
}

.movie-type {
  font-size: 24rpx;
  color: #666;
  margin-bottom: 8rpx;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.movie-actors {
  font-size: 24rpx;
  color: #999;
  margin-bottom: 16rpx;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.movie-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.movie-count {
  font-size: 24rpx;
  color: #ff9900;
}

.movie-date {
  font-size: 24rpx;
  color: #666;
}

.buy-btn {
  background-color: #ff4d4f;
  padding: 12rpx 32rpx;
  border-radius: 8rpx;
}

.buy-btn text {
  font-size: 26rpx;
  color: #fff;
}

.presell-btn {
  background-color: #ff9900;
  padding: 12rpx 32rpx;
  border-radius: 8rpx;
}

.presell-btn text {
  font-size: 26rpx;
  color: #fff;
}
</style>
