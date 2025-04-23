<template>
  <view class="home-container">
    <!-- 顶部导航栏 -->
    <view class="nav-header">
      <view class="location-info">
        <image class="logo" src="/static/images/logo.png" mode="aspectFit"></image>
        <text class="city-name">舌尖州</text>
        <text class="badge">里</text>
      </view>
      <view class="search-bar">
        <view class="search-input">
          <text class="search-placeholder">搜索地方美食</text>
          <image class="search-icon" src="/static/images/search.png" mode="aspectFit"></image>
        </view>
        <text class="collect-btn">收藏</text>
      </view>
      <view class="category-tabs">
        <view 
          v-for="(tab, index) in tabs" 
          :key="index"
          :class="['tab-item', { active: currentTab === index }]"
          @tap="switchTab(index)"
        >
          {{ tab.name }}
        </view>
      </view>
    </view>

    <!-- 主要内容区域 -->
    <view class="content-area">
      <!-- 标题横幅 -->
      <view class="banner">
        <view class="banner-content">
          <image class="cloud-left" src="/static/images/cloud.png" mode="aspectFit"></image>
          <view class="banner-title">
            <text class="title-text">舌尖上的九州</text>
            <text class="subtitle-text">且将新火试新茶</text>
          </view>
          <image class="cloud-right" src="/static/images/cloud.png" mode="aspectFit"></image>
        </view>
      </view>

      <!-- 美食地图展示区 -->
      <view class="food-showcase">
        <view class="map-container">
          <image class="china-map" src="/static/images/china-map.png" mode="aspectFit"></image>
          <!-- 地区标记点 -->
          <view 
            v-for="(region, index) in regions" 
            :key="index"
            :class="['region-marker', { active: selectedRegion === region.id }]"
            :style="{ left: region.position.x + '%', top: region.position.y + '%' }"
            @tap="selectRegion(region)"
          >
            <view class="marker-dot"></view>
            <text class="region-name">{{ region.name }}</text>
          </view>
          
          <!-- 美食展示卡片 -->
          <view v-if="selectedRegion" class="food-card" :style="foodCardStyle">
            <view class="card-header">
              <text class="region-title">{{ selectedRegionData.name }}特色美食</text>
              <text class="close-btn" @tap="closeCard">×</text>
            </view>
            <scroll-view class="food-list" scroll-y>
              <view 
                v-for="(food, index) in selectedRegionData.foods" 
                :key="index"
                class="food-item"
                @tap="showFoodDetail(food)"
              >
                <image :src="food.image" mode="aspectFill" class="food-image"></image>
                <view class="food-info">
                  <text class="food-name">{{ food.name }}</text>
                  <text class="food-desc">{{ food.description }}</text>
                </view>
              </view>
            </scroll-view>
          </view>
          
          <!-- 熊猫装饰元素 -->
          <view class="panda-decoration">
            <image class="panda-item umbrella" src="/static/images/panda-umbrella.png" mode="aspectFit"></image>
            <image class="panda-item hat" src="/static/images/panda-hat.png" mode="aspectFit"></image>
            <image class="panda-item fire" src="/static/images/panda-fire.png" mode="aspectFit"></image>
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
      currentTab: 0,
      selectedRegion: null,
      tabs: [
        { name: '随机推荐', id: 'random' },
        { name: '收藏', id: 'collection' },
        { name: '精选', id: 'featured' },
        { name: '重庆', id: 'chongqing' },
        { name: '食宗', id: 'food-sect' },
        { name: '时精', id: 'seasonal' },
        { name: '34⌒', id: '34' },
        { name: '食地区', id: 'regions' }
      ],
      regions: [
        {
          id: 'sichuan',
          name: '四川',
          position: { x: 45, y: 55 },
          foods: [
            {
              name: '回锅肉',
              image: '/static/images/food/huiguorou.png',
              description: '川菜代表菜品，咸鲜麻辣',
              price: '￥38'
            },
            {
              name: '麻婆豆腐',
              image: '/static/images/food/mapo.png',
              description: '经典川菜，麻辣鲜香',
              price: '￥28'
            }
          ]
        },
        {
          id: 'guangdong',
          name: '广东',
          position: { x: 55, y: 75 },
          foods: [
            {
              name: '叉烧',
              image: '/static/images/food/char-siu.png',
              description: '广式烧腊，甜咸适中',
              price: '￥48'
            }
          ]
        },
        {
          id: 'beijing',
          name: '北京',
          position: { x: 65, y: 35 },
          foods: [
            {
              name: '烤鸭',
              image: '/static/images/food/kaoya.png',
              description: '北京特色美食，外脆里嫩',
              price: '￥188'
            }
          ]
        }
      ]
    }
  },
  computed: {
    selectedRegionData() {
      return this.regions.find(r => r.id === this.selectedRegion) || null;
    },
    foodCardStyle() {
      if (!this.selectedRegionData) return {};
      const { x, y } = this.selectedRegionData.position;
      return {
        left: `${x}%`,
        top: `${y}%`
      };
    }
  },
  methods: {
    switchTab(index) {
      this.currentTab = index;
    },
    selectRegion(region) {
      this.selectedRegion = region.id;
    },
    closeCard() {
      this.selectedRegion = null;
    },
    showFoodDetail(food) {
      uni.showToast({
        title: `选择了${food.name}`,
        icon: 'none'
      });
    }
  }
}
</script>

<style lang="scss">
.home-container {
  min-height: 100vh;
  background-color: #013440;
  color: #fff;
}

/* 顶部导航样式 */
.nav-header {
  padding: 20rpx 30rpx;
  background: linear-gradient(to bottom, #013440, #024955);
}

.location-info {
  display: flex;
  align-items: center;
  margin-bottom: 20rpx;
  
  .logo {
    width: 60rpx;
    height: 60rpx;
    margin-right: 10rpx;
  }
  
  .city-name {
    font-size: 32rpx;
    font-weight: bold;
  }
  
  .badge {
    margin-left: 10rpx;
    background-color: #d4713f;
    padding: 4rpx 12rpx;
    border-radius: 20rpx;
    font-size: 24rpx;
  }
}

.search-bar {
  display: flex;
  align-items: center;
  margin-bottom: 20rpx;
  
  .search-input {
    flex: 1;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 30rpx;
    height: 70rpx;
    display: flex;
    align-items: center;
    padding: 0 30rpx;
    margin-right: 20rpx;
    
    .search-placeholder {
      color: rgba(255, 255, 255, 0.6);
      font-size: 28rpx;
    }
    
    .search-icon {
      width: 40rpx;
      height: 40rpx;
      margin-left: auto;
    }
  }
  
  .collect-btn {
    background: linear-gradient(135deg, #f8b551, #f89251);
    padding: 10rpx 30rpx;
    border-radius: 30rpx;
    font-size: 28rpx;
  }
}

.category-tabs {
  display: flex;
  overflow-x: auto;
  white-space: nowrap;
  margin: 0 -30rpx;
  padding: 0 30rpx;
  
  &::-webkit-scrollbar {
    display: none;
  }
  
  .tab-item {
    padding: 15rpx 30rpx;
    font-size: 28rpx;
    color: rgba(255, 255, 255, 0.7);
    position: relative;
    
    &.active {
      color: #fff;
      font-weight: bold;
      
      &::after {
        content: '';
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        width: 40rpx;
        height: 4rpx;
        background-color: #f8b551;
        border-radius: 2rpx;
      }
    }
  }
}

/* 内容区域样式 */
.content-area {
  padding: 30rpx;
}

.banner {
  margin-bottom: 40rpx;
  
  .banner-content {
    background: url('/static/images/banner-bg.png') no-repeat center/cover;
    height: 200rpx;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    border-radius: 20rpx;
    overflow: hidden;
    
    .cloud-left,
    .cloud-right {
      position: absolute;
      width: 120rpx;
      height: 60rpx;
      opacity: 0.8;
    }
    
    .cloud-left {
      left: 40rpx;
      transform: scaleX(-1);
    }
    
    .cloud-right {
      right: 40rpx;
    }
    
    .banner-title {
      text-align: center;
      
      .title-text {
        font-size: 48rpx;
        font-weight: bold;
        color: #f8d3a7;
        text-shadow: 0 2rpx 4rpx rgba(0, 0, 0, 0.3);
        display: block;
        margin-bottom: 10rpx;
      }
      
      .subtitle-text {
        font-size: 28rpx;
        color: #f8d3a7;
        opacity: 0.8;
      }
    }
  }
}

.food-showcase {
  position: relative;
  
  .scroll-container {
    overflow: hidden;
    border-radius: 20rpx;
  }
  
  .food-map {
    position: relative;
    width: 100%;
    padding-bottom: 100%;
    background: #fff;
    
    .map-bg {
      position: absolute;
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
  }
}

.food-item {
  position: absolute;
  width: 120rpx;
  height: 120rpx;
  cursor: pointer;
  transition: transform 0.3s;
  
  &:hover {
    transform: scale(1.1);
  }
  
  .food-image {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }
  
  .food-name {
    position: absolute;
    bottom: -30rpx;
    left: 50%;
    transform: translateX(-50%);
    font-size: 24rpx;
    color: #333;
    white-space: nowrap;
    background: rgba(255, 255, 255, 0.9);
    padding: 4rpx 12rpx;
    border-radius: 10rpx;
  }
}

.panda-decoration {
  position: absolute;
  width: 100%;
  height: 100%;
  pointer-events: none;
  
  .panda-item {
    position: absolute;
    width: 100rpx;
    height: 100rpx;
    object-fit: contain;
    
    &.umbrella {
      top: 10%;
      left: 10%;
      animation: float 3s ease-in-out infinite;
    }
    
    &.hat {
      top: 20%;
      right: 15%;
      animation: float 3s ease-in-out infinite 1s;
    }
    
    &.fire {
      bottom: 15%;
      right: 10%;
      animation: float 3s ease-in-out infinite 2s;
    }
  }
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10rpx);
  }
}

/* 新增地图相关样式 */
.map-container {
  position: relative;
  width: 100%;
  padding-bottom: 75%; /* 4:3 比例 */
  background: #fff;
  border-radius: 20rpx;
  overflow: hidden;
}

.china-map {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.region-marker {
  position: absolute;
  transform: translate(-50%, -50%);
  cursor: pointer;
  z-index: 2;
  
  .marker-dot {
    width: 20rpx;
    height: 20rpx;
    background: #f8b551;
    border-radius: 50%;
    box-shadow: 0 0 0 8rpx rgba(248, 181, 81, 0.2);
    animation: pulse 2s infinite;
  }
  
  .region-name {
    position: absolute;
    left: 50%;
    bottom: -30rpx;
    transform: translateX(-50%);
    font-size: 24rpx;
    color: #333;
    white-space: nowrap;
    background: rgba(255, 255, 255, 0.9);
    padding: 4rpx 12rpx;
    border-radius: 10rpx;
  }
  
  &.active .marker-dot {
    background: #d4713f;
    box-shadow: 0 0 0 8rpx rgba(212, 113, 63, 0.2);
  }
}

@keyframes pulse {
  0% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.2);
    opacity: 0.8;
  }
  100% {
    transform: scale(1);
    opacity: 1;
  }
}

.food-card {
  position: absolute;
  width: 600rpx;
  max-height: 800rpx;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 20rpx;
  box-shadow: 0 4rpx 20rpx rgba(0, 0, 0, 0.15);
  z-index: 3;
  transform: translate(-50%, -120%);
  
  .card-header {
    padding: 20rpx;
    border-bottom: 2rpx solid #eee;
    display: flex;
    justify-content: space-between;
    align-items: center;
    
    .region-title {
      font-size: 32rpx;
      font-weight: bold;
      color: #333;
    }
    
    .close-btn {
      font-size: 40rpx;
      color: #999;
      padding: 0 20rpx;
      cursor: pointer;
    }
  }
  
  .food-list {
    max-height: 600rpx;
    padding: 20rpx;
  }
  
  .food-item {
    display: flex;
    padding: 20rpx;
    border-bottom: 2rpx solid #f5f5f5;
    
    &:last-child {
      border-bottom: none;
    }
    
    .food-image {
      width: 160rpx;
      height: 160rpx;
      border-radius: 12rpx;
      margin-right: 20rpx;
    }
    
    .food-info {
      flex: 1;
      
      .food-name {
        font-size: 28rpx;
        color: #333;
        font-weight: bold;
        margin-bottom: 10rpx;
      }
      
      .food-desc {
        font-size: 24rpx;
        color: #666;
        line-height: 1.4;
      }
    }
  }
}
</style>
