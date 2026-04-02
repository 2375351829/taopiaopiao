<template>
  <view class="index">
    <!-- 轮播图 -->
    <swiper class="swiper" indicator-dots autoplay interval="3000" duration="500">
      <swiper-item v-for="(item, index) in banners" :key="index">
        <image :src="item.image" class="swiper-image" mode="aspectFill"></image>
      </swiper-item>
    </swiper>

    <!-- 快捷入口 -->
    <view class="quick-entry">
      <view class="entry-item" v-for="(item, index) in quickEntries" :key="index">
        <image :src="item.icon" class="entry-icon"></image>
        <text class="entry-text">{{ item.text }}</text>
      </view>
    </view>

    <!-- 热门电影 -->
    <view class="hot-movies">
      <view class="section-header">
        <text class="section-title">热门电影</text>
        <navigator url="/pages/movie/list" class="more">更多 ></navigator>
      </view>
      <view class="movie-list">
        <view class="movie-item" v-for="(movie, index) in hotMovies" :key="index" @click="goToMovieDetail(movie.id)">
          <image :src="movie.poster" class="movie-poster"></image>
          <text class="movie-title">{{ movie.title }}</text>
          <text class="movie-score">{{ movie.score }}</text>
        </view>
      </view>
    </view>

    <!-- 热门活动 -->
    <view class="hot-activities">
      <view class="section-header">
        <text class="section-title">热门活动</text>
        <navigator url="" class="more">更多 ></navigator>
      </view>
      <view class="activity-list">
        <view class="activity-item" v-for="(activity, index) in activities" :key="index">
          <image :src="activity.image" class="activity-image"></image>
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
        {
          image: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=movie%20banner%20poster%202026&image_size=landscape_16_9"
        },
        {
          image: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=movie%20promotion%20banner%202026&image_size=landscape_16_9"
        },
        {
          image: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=cinema%20promotion%20banner%202026&image_size=landscape_16_9"
        }
      ],
      quickEntries: [
        { icon: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=movie%20icon&image_size=square", text: "热映"
        },
        { icon: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=coming%20soon%20icon&image_size=square", text: "即将上映" },
        { icon: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=cinema%20icon&image_size=square", text: "影院" },
        { icon: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=activity%20icon&image_size=square", text: "活动" }
      ],
      hotMovies: [
        {
          id: 1,
          title: "阿凡达3",
          score: "9.2",
          poster: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=Avatar%203%20movie%20poster%202026&image_size=portrait_4_3"
        },
        {
          id: 2,
          title: "复仇者联盟5",
          score: "8.8",
          poster: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=Avengers%205%20movie%20poster%202026&image_size=portrait_4_3"
        },
        {
          id: 3,
          title: "星球大战10",
          score: "8.5",
          poster: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=Star%20Wars%2010%20movie%20poster%202026&image_size=portrait_4_3"
        },
        {
          id: 4,
          title: "流浪地球3",
          score: "9.0",
          poster: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=The%20Wandering%20Earth%203%20movie%20poster%202026&image_size=portrait_4_3"
        }
      ],
      activities: [
        {
          image: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=movie%20activity%20banner%20discount&image_size=landscape_4_3"
        },
        {
          image: "https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=movie%20activity%20banner%20membership&image_size=landscape_4_3"
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
.index {
  padding-bottom: 50px;
}

.swiper {
  width: 100%;
  height: 200px;
}

.swiper-image {
  width: 100%;
  height: 100%;
}

.quick-entry {
  display: flex;
  justify-content: space-around;
  padding: 20px 0;
  background-color: #fff;
  margin-bottom: 10px;
}

.entry-item {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.entry-icon {
  width: 48px;
  height: 48px;
  margin-bottom: 8px;
}

.entry-text {
  font-size: 12px;
  color: #333;
}

.hot-movies,
.hot-activities {
  background-color: #fff;
  margin-bottom: 10px;
  padding: 15px;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.section-title {
  font-size: 16px;
  font-weight: bold;
  color: #333;
}

.more {
  font-size: 12px;
  color: #999;
}

.movie-list {
  display: flex;
  overflow-x: auto;
  padding-bottom: 10px;
}

.movie-item {
  flex-shrink: 0;
  width: 100px;
  margin-right: 15px;
}

.movie-poster {
  width: 100px;
  height: 140px;
  border-radius: 4px;
  margin-bottom: 8px;
}

.movie-title {
  font-size: 12px;
  color: #333;
  margin-bottom: 4px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.movie-score {
  font-size: 12px;
  color: #ff4d4f;
}

.activity-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.activity-image {
  width: 100%;
  height: 100px;
  border-radius: 4px;
}
</style>