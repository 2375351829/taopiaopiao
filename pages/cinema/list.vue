<template>
  <view class="cinema-list">
    <!-- 标签切换 -->
    <view class="tab-bar">
      <view class="tab-item" :class="{ active: activeTab === 'nearby' }" @click="activeTab = 'nearby'">
        <text>附近影院</text>
      </view>
      <view class="tab-item" :class="{ active: activeTab === 'hot' }" @click="activeTab = 'hot'">
        <text>热门影院</text>
      </view>
    </view>

    <!-- 影院列表 -->
    <view class="cinemas-container">
      <!-- 附近影院 -->
      <view v-if="activeTab === 'nearby'" class="cinemas">
        <view class="cinema-item" v-for="(cinema, index) in nearbyCinemas" :key="index">
          <view class="cinema-info">
            <text class="cinema-name">{{ cinema.name }}</text>
            <text class="cinema-address">{{ cinema.address }}</text>
            <view class="cinema-features">
              <text class="feature" v-for="(feature, featureIndex) in cinema.features" :key="featureIndex">{{ feature }}</text>
            </view>
          </view>
          <view class="cinema-distance">
            <text class="distance">{{ cinema.distance }}</text>
          </view>
        </view>
      </view>

      <!-- 热门影院 -->
      <view v-else class="cinemas">
        <view class="cinema-item" v-for="(cinema, index) in hotCinemas" :key="index">
          <view class="cinema-info">
            <text class="cinema-name">{{ cinema.name }}</text>
            <text class="cinema-address">{{ cinema.address }}</text>
            <view class="cinema-features">
              <text class="feature" v-for="(feature, featureIndex) in cinema.features" :key="featureIndex">{{ feature }}</text>
            </view>
          </view>
          <view class="cinema-rating">
            <text class="rating">{{ cinema.rating }}</text>
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
      activeTab: 'nearby',
      nearbyCinemas: [
        {
          id: 1,
          name: "万达影城(北京CBD店)",
          address: "北京市朝阳区建国路87号",
          distance: "500m",
          features: ["IMAX", "4D", "杜比全景声"]
        },
        {
          id: 2,
          name: "CGV影城(北京五道口店)",
          address: "北京市海淀区成府路28号",
          distance: "1.2km",
          features: ["IMAX", "4D"]
        },
        {
          id: 3,
          name: "UME影城(北京华星店)",
          address: "北京市海淀区中关村大街15号",
          distance: "2.5km",
          features: ["IMAX", "杜比全景声"]
        },
        {
          id: 4,
          name: "保利国际影城(北京望京店)",
          address: "北京市朝阳区望京广顺北大街33号",
          distance: "3.8km",
          features: ["4D", "杜比全景声"]
        }
      ],
      hotCinemas: [
        {
          id: 1,
          name: "万达影城(北京CBD店)",
          address: "北京市朝阳区建国路87号",
          rating: "4.8",
          features: ["IMAX", "4D", "杜比全景声"]
        },
        {
          id: 2,
          name: "CGV影城(北京五道口店)",
          address: "北京市海淀区成府路28号",
          rating: "4.7",
          features: ["IMAX", "4D"]
        },
        {
          id: 3,
          name: "UME影城(北京华星店)",
          address: "北京市海淀区中关村大街15号",
          rating: "4.6",
          features: ["IMAX", "杜比全景声"]
        },
        {
          id: 4,
          name: "保利国际影城(北京望京店)",
          address: "北京市朝阳区望京广顺北大街33号",
          rating: "4.5",
          features: ["4D", "杜比全景声"]
        },
        {
          id: 5,
          name: "金逸影城(北京朝阳大悦城店)",
          address: "北京市朝阳区朝阳北路101号",
          rating: "4.4",
          features: ["IMAX", "4D"]
        }
      ]
    }
  }
}
</script>

<style scoped>
.cinema-list {
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

.cinemas-container {
  padding: 10px;
}

.cinemas {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.cinema-item {
  display: flex;
  background-color: #fff;
  border-radius: 8px;
  padding: 15px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.cinema-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.cinema-name {
  font-size: 16px;
  font-weight: bold;
  color: #333;
}

.cinema-address {
  font-size: 14px;
  color: #666;
  line-height: 1.4;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.cinema-features {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.feature {
  font-size: 12px;
  color: #666;
  background-color: #f0f0f0;
  padding: 2px 8px;
  border-radius: 4px;
}

.cinema-distance,
.cinema-rating {
  display: flex;
  align-items: flex-start;
  margin-left: 15px;
}

.distance,
.rating {
  font-size: 14px;
  color: #ff4d4f;
  font-weight: bold;
}
</style>