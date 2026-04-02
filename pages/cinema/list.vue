<template>
  <view class="cinema-list">
    <view class="tab-bar">
      <view class="tab-item" :class="{ active: activeTab === 'nearby' }" @click="activeTab = 'nearby'">
        <text>附近影院</text>
      </view>
      <view class="tab-item" :class="{ active: activeTab === 'hot' }" @click="activeTab = 'hot'">
        <text>热门影院</text>
      </view>
    </view>

    <view class="map-container">
      <map
        class="cinema-map"
        :latitude="centerLatitude"
        :longitude="centerLongitude"
        :markers="markers"
        :scale="mapScale"
        show-location
      ></map>
      <view class="map-info">
        <text class="map-title">重庆外语外事学院</text>
        <text class="map-address">重庆市渝北区回兴街道宝桐路4号</text>
      </view>
    </view>

    <view class="cinemas-container">
      <view v-if="activeTab === 'nearby'" class="cinemas">
        <view class="cinema-item" v-for="(cinema, index) in nearbyCinemas" :key="index" @click="goToCinema(cinema)">
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

      <view v-else class="cinemas">
        <view class="cinema-item" v-for="(cinema, index) in hotCinemas" :key="index" @click="goToCinema(cinema)">
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
      centerLatitude: 29.664563,
      centerLongitude: 106.607912,
      mapScale: 14,
      markers: [
        {
          id: 1,
          latitude: 29.664563,
          longitude: 106.607912,
          title: "重庆外语外事学院",
          width: 40,
          height: 40,
          callout: {
            content: "重庆外语外事学院",
            color: "#333",
            fontSize: 14,
            borderRadius: 10,
            padding: 10,
            display: "ALWAYS"
          }
        }
      ],
      nearbyCinemas: [
        { id: 1, name: "万达影城(重庆渝北店)", address: "重庆市渝北区双龙大道100号", distance: "1.2km", features: ["IMAX", "4D", "杜比全景声"], latitude: 29.669563, longitude: 106.617912 },
        { id: 2, name: "CGV影城(重庆金港国际店)", address: "重庆市渝北区金港国际购物中心", distance: "2.5km", features: ["IMAX", "4D"], latitude: 29.674563, longitude: 106.627912 },
        { id: 3, name: "UME影城(重庆加州店)", address: "重庆市渝北区加州花园", distance: "3.8km", features: ["IMAX", "杜比全景声"], latitude: 29.684563, longitude: 106.637912 },
        { id: 4, name: "保利国际影城(重庆星光天地店)", address: "重庆市渝北区星光天地L3层", distance: "4.5km", features: ["4D", "杜比全景声"], latitude: 29.694563, longitude: 106.647912 }
      ],
      hotCinemas: [
        { id: 1, name: "万达影城(重庆渝北店)", address: "重庆市渝北区双龙大道100号", rating: "4.8", features: ["IMAX", "4D", "杜比全景声"], latitude: 29.669563, longitude: 106.617912 },
        { id: 2, name: "CGV影城(重庆金港国际店)", address: "重庆市渝北区金港国际购物中心", rating: "4.7", features: ["IMAX", "4D"], latitude: 29.674563, longitude: 106.627912 },
        { id: 3, name: "UME影城(重庆加州店)", address: "重庆市渝北区加州花园", rating: "4.6", features: ["IMAX", "杜比全景声"], latitude: 29.684563, longitude: 106.637912 },
        { id: 4, name: "保利国际影城(重庆星光天地店)", address: "重庆市渝北区星光天地L3层", rating: "4.5", features: ["4D", "杜比全景声"], latitude: 29.694563, longitude: 106.647912 },
        { id: 5, name: "金逸影城(重庆龙湖时代天街店)", address: "重庆市渝中区龙湖时代天街", rating: "4.4", features: ["IMAX", "4D"], latitude: 29.654563, longitude: 106.597912 }
      ]
    }
  },
  methods: {
    goToCinema(cinema) {
      if (!cinema || !cinema.id) {
        uni.showToast({ title: '影院信息异常', icon: 'none' });
        return;
      }
      uni.navigateTo({
        url: `/pages/ticket/buy?cinemaId=${cinema.id}&cinemaName=${encodeURIComponent(cinema.name || '')}&cinemaAddress=${encodeURIComponent(cinema.address || '')}&latitude=${cinema.latitude || 0}&longitude=${cinema.longitude || 0}`
      })
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

.map-container {
  position: relative;
  background-color: #fff;
  margin-bottom: 20rpx;
}

.cinema-map {
  width: 100%;
  height: 350rpx;
}

.map-info {
  padding: 20rpx 30rpx;
  border-top: 1px solid #eee;
}

.map-title {
  font-size: 30rpx;
  font-weight: bold;
  color: #333;
  display: block;
  margin-bottom: 8rpx;
}

.map-address {
  font-size: 24rpx;
  color: #666;
  display: block;
}

.cinemas-container {
  padding: 20rpx;
}

.cinemas {
  display: flex;
  flex-direction: column;
  gap: 20rpx;
}

.cinema-item {
  display: flex;
  background-color: #fff;
  border-radius: 16rpx;
  padding: 30rpx;
}

.cinema-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 12rpx;
  min-width: 0;
}

.cinema-name {
  font-size: 32rpx;
  font-weight: bold;
  color: #333;
}

.cinema-address {
  font-size: 24rpx;
  color: #666;
  line-height: 1.4;
}

.cinema-features {
  display: flex;
  flex-wrap: wrap;
  gap: 12rpx;
}

.feature {
  font-size: 22rpx;
  color: #666;
  background-color: #f0f0f0;
  padding: 4rpx 16rpx;
  border-radius: 8rpx;
}

.cinema-distance,
.cinema-rating {
  display: flex;
  align-items: flex-start;
  margin-left: 30rpx;
  flex-shrink: 0;
}

.distance,
.rating {
  font-size: 28rpx;
  color: #ff4d4f;
  font-weight: bold;
}
</style>
