<template>
  <view class="movie-list">
    <!-- 标签切换 -->
    <view class="tab-bar">
      <view class="tab-item" :class="{ active: activeTab === 'hot' }" @click="activeTab = 'hot'">
        <text>热映</text>
      </view>
      <view class="tab-item" :class="{ active: activeTab === 'coming' }" @click="activeTab = 'coming'">
        <text>即将上映</text>
      </view>
    </view>

    <!-- 电影列表 -->
    <view class="movies-container">
      <!-- 热映电影 -->
      <view v-if="activeTab === 'hot'" class="movies">
        <view class="movie-item" v-for="(movie, index) in hotMovies" :key="index" @click="goToMovieDetail(movie.id)">
          <image :src="movie.poster" class="movie-poster"></image>
          <view class="movie-info">
            <text class="movie-title">{{ movie.title }}</text>
            <text class="movie-score">{{ movie.score }}</text>
            <text class="movie-desc">{{ movie.desc }}</text>
            <view class="movie-tags">
              <text class="tag" v-for="(tag, tagIndex) in movie.tags" :key="tagIndex">{{ tag }}</text>
            </view>
          </view>
        </view>
      </view>

      <!-- 即将上映电影 -->
      <view v-else class="movies">
        <view class="movie-item" v-for="(movie, index) in comingMovies" :key="index" @click="goToMovieDetail(movie.id)">
          <image :src="movie.poster" class="movie-poster"></image>
          <view class="movie-info">
            <text class="movie-title">{{ movie.title }}</text>
            <text class="movie-date">{{ movie.releaseDate }}</text>
            <text class="movie-desc">{{ movie.desc }}</text>
            <view class="movie-tags">
              <text class="tag" v-for="(tag, tagIndex) in movie.tags" :key="tagIndex">{{ tag }}</text>
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
        {
          id: 1,
          title: "阿凡达3",
          score: "9.2",
          desc: "詹姆斯·卡梅隆执导的科幻巨制，讲述人类与纳美人的冲突与和解。",
          tags: ["科幻", "冒险", "3D"],
          poster: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=Avatar%203%20movie%20poster%202026&image_size=portrait_4_3"
        },
        {
          id: 2,
          title: "复仇者联盟5",
          score: "8.8",
          desc: "漫威超级英雄集结，共同对抗强大的反派。",
          tags: ["动作", "科幻", "冒险"],
          poster: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=Avengers%205%20movie%20poster%202026&image_size=portrait_4_3"
        },
        {
          id: 3,
          title: "星球大战10",
          score: "8.5",
          desc: "星球大战系列的最新续作，讲述新的冒险故事。",
          tags: ["科幻", "动作", "太空"],
          poster: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=Star%20Wars%2010%20movie%20poster%202026&image_size=portrait_4_3"
        },
        {
          id: 4,
          title: "流浪地球3",
          score: "9.0",
          desc: "中国科幻电影的里程碑，讲述人类带着地球寻找新家园的故事。",
          tags: ["科幻", "灾难", "中国"],
          poster: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=The%20Wandering%20Earth%203%20movie%20poster%202026&image_size=portrait_4_3"
        }
      ],
      comingMovies: [
        {
          id: 5,
          title: "银河护卫队4",
          releaseDate: "2026-05-01",
          desc: "银河护卫队的新冒险，充满幽默和动作。",
          tags: ["科幻", "动作", "喜剧"],
          poster: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=Guardians%20of%20the%20Galaxy%204%20movie%20poster%202026&image_size=portrait_4_3"
        },
        {
          id: 6,
          title: "变形金刚7",
          releaseDate: "2026-06-15",
          desc: "变形金刚系列的最新作品，机器人之间的战争继续。",
          tags: ["科幻", "动作", "机器人"],
          poster: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=Transformers%207%20movie%20poster%202026&image_size=portrait_4_3"
        },
        {
          id: 7,
          title: "蜘蛛侠：新纪元",
          releaseDate: "2026-07-20",
          desc: "蜘蛛侠的新冒险，跨越多个平行宇宙。",
          tags: ["动画", "科幻", "冒险"],
          poster: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=Spider-Man%20New%20Era%20movie%20poster%202026&image_size=portrait_4_3"
        }
      ]
    }
  },
  methods: {
    goToMovieDetail(movieId) {
      uni.navigateTo({
        url: `/pages/movie/detail?id=${movieId}`
      })
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
}

.tab-item {
  flex: 1;
  text-align: center;
  padding: 15px 0;
  position: relative;
}

.tab-item.active {
  color: #ff4d4f;
}

.tab-item.active::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 25%;
  width: 50%;
  height: 2px;
  background-color: #ff4d4f;
}

.movies-container {
  padding: 10px;
}

.movies {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.movie-item {
  display: flex;
  background-color: #fff;
  border-radius: 8px;
  padding: 15px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.movie-poster {
  width: 100px;
  height: 140px;
  border-radius: 4px;
  margin-right: 15px;
}

.movie-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.movie-title {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin-bottom: 8px;
}

.movie-score {
  font-size: 14px;
  color: #ff4d4f;
  margin-bottom: 8px;
}

.movie-date {
  font-size: 14px;
  color: #666;
  margin-bottom: 8px;
}

.movie-desc {
  font-size: 12px;
  color: #999;
  line-height: 1.4;
  margin-bottom: 10px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
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
</style>