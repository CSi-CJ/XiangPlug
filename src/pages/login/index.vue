<template>
  <view class="login-container">
    <!-- 背景与动画元素 -->
    <view class="background-image">
      <view class="food-landscape">
        <view class="food-mountains">
          <view class="mountain mountain-1"></view>
          <view class="mountain mountain-2"></view>
          <view class="mountain mountain-3"></view>
        </view>
        
        <!-- 动态食物元素 -->
        <view class="food-item chicken"></view>
        <view class="food-item fish"></view>
        <view class="food-item duck"></view>
        <view class="food-item pork"></view>
        <view class="food-item food-bowl"></view>
        
        <!-- 小人动画 -->
        <view class="traveler"></view>
        <view class="traveler traveler-2"></view>
        <view class="traveler traveler-3"></view>
      </view>
    </view>
    
    <!-- 头部标题区域 -->
    <view class="header">
      <view class="title-wrapper">
        <text class="title">人间烟火美食</text>
        <view class="logo-stamp"></view>
        <text class="subtitle">发现家乡美食，分享舌尖故事</text>
      </view>
    </view>
    
    <!-- 登录表单 -->
    <view class="login-form">
      <view class="form-group">
        <input 
          type="number" 
          class="form-input" 
          :focus="phoneFocus"
          placeholder=" " 
          v-model="phone"
          @focus="phoneFocus = true"
          @blur="phoneFocus = false"
          maxlength="11"
        />
        <label :class="['form-label', {active: phoneFocus || phone}]">手机号</label>
      </view>
      
      <view class="form-group">
        <input 
          type="number" 
          class="form-input" 
          :focus="codeFocus"
          placeholder=" " 
          v-model="verifyCode"
          @focus="codeFocus = true"
          @blur="codeFocus = false"
          maxlength="6"
        />
        <label :class="['form-label', {active: codeFocus || verifyCode}]">验证码</label>
      </view>
      
      <button 
        class="btn-login"
        :class="{'countdown-active': isCountingDown}" 
        :disabled="isLoading || isCountingDown || !phone" 
        @click="getVerifyCode"
      >{{ btnText }}</button>
      
      <button 
        class="btn-login mt-15" 
        :disabled="isLoading || !phone || !verifyCode" 
        @click="handleLogin"
      >登录</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      phone: '',
      verifyCode: '',
      btnText: '获取验证码',
      isLoading: false,
      isCountingDown: false,
      countdown: 60,
      timer: null,
      phoneFocus: false,
      codeFocus: false
    }
  },
  onUnload() {
    // 清除计时器
    if (this.timer) {
      clearInterval(this.timer);
      this.timer = null;
    }
  },
  methods: {
    // 获取验证码
    getVerifyCode() {
      if (this.isLoading || this.isCountingDown) return;
      
      if (!this.validatePhone()) {
        uni.showToast({
          title: '请输入正确的手机号',
          icon: 'none'
        });
        return;
      }
      
      this.isLoading = true;
      
      // 模拟请求发送验证码
      console.log('模拟发送验证码到:', this.phone);
      setTimeout(() => {
        this.isLoading = false;
        this.startCountdown();
        uni.showToast({
          title: '验证码已发送',
          icon: 'success'
        });
      }, 1000);
    },
    
    // 倒计时
    startCountdown() {
      this.isCountingDown = true;
      this.countdown = 60;
      this.btnText = `${this.countdown}秒后重试`;
      
      if (this.timer) {
        clearInterval(this.timer);
      }
      
      this.timer = setInterval(() => {
        this.countdown--;
        this.btnText = `${this.countdown}秒后重试`;
        
        if (this.countdown <= 0) {
          clearInterval(this.timer);
          this.timer = null;
          this.btnText = '获取验证码';
          this.isCountingDown = false;
        }
      }, 1000);
    },
    
    // 登录处理
    handleLogin() {
      if (this.isLoading) return;
      
      if (!this.validatePhone()) {
        uni.showToast({
          title: '请输入正确的手机号',
          icon: 'none'
        });
        return;
      }
      
      if (!this.verifyCode || this.verifyCode.length !== 6) {
        uni.showToast({
          title: '请输入6位验证码',
          icon: 'none'
        });
        return;
      }
      
      this.isLoading = true;
      
      // 模拟登录请求
      console.log('模拟登录，手机号:', this.phone, '验证码:', this.verifyCode);
      setTimeout(() => {
        this.isLoading = false;
        // 假设登录成功
        uni.showToast({
          title: '登录成功',
          icon: 'success'
        });
        
        // 存储用户信息示例
        try {
          uni.setStorageSync('token', 'mock_token_' + Date.now());
          uni.setStorageSync('userInfo', JSON.stringify({ phone: this.phone, nickname: '美食家' }));
        } catch (e) {
          console.error('存储失败:', e);
        }
        
        // 登录成功后跳转到首页
        setTimeout(() => {
          uni.switchTab({
            url: '/pages/index/index'
          });
        }, 1500);
      }, 1000);
    },
    
    // 验证手机号
    validatePhone() {
      return /^1[3-9]\d{9}$/.test(this.phone);
    }
  }
}
</script>

<style lang="scss">
page {
  height: 100%;
  background-color: #fff8e7;
}

.login-container {
  position: relative;
  width: 100%;
  min-height: 100vh;
  padding: 40rpx;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  overflow: hidden;
}

/* 背景与动画 */
.background-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
}

.food-landscape {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(180deg, rgba(255,175,75,0.8) 0%, rgba(255,204,145,0.3) 100%);
  overflow: hidden;
  
  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: 
      radial-gradient(circle at 30% 30%, rgba(255,220,165,0.6) 0%, rgba(0,0,0,0) 50%),
      radial-gradient(circle at 70% 60%, rgba(255,235,205,0.6) 0%, rgba(0,0,0,0) 40%);
  }
}

/* 小人动画 */
.traveler {
  position: absolute;
  width: 60rpx;
  height: 36rpx;
  bottom: 360rpx;
  animation: travel 30s linear infinite;
  z-index: 2;
  
  &::before {
    content: '';
    position: absolute;
    width: 16rpx;
    height: 12rpx;
    background-color: #8b4513;
    border-radius: 50% 50% 0 0;
    top: 0;
    left: 22rpx;
  }
  
  &::after {
    content: '';
    position: absolute;
    width: 60rpx;
    height: 24rpx;
    background-color: #d4a373;
    border-radius: 40% 40% 20% 20%;
    bottom: 0;
    left: 0;
  }
}

.traveler-2 {
  bottom: 460rpx;
  animation-delay: -10s;
  transform: scale(0.9);
}

.traveler-3 {
  bottom: 560rpx;
  animation-delay: -20s;
  transform: scale(0.7);
}

@keyframes travel {
  0% {
    left: -60rpx;
  }
  100% {
    left: calc(100% + 60rpx);
  }
}

/* 食物动画元素 */
.food-item {
  position: absolute;
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
  animation: float 6s ease-in-out infinite;
  z-index: 2;
}

.chicken {
  width: 100rpx;
  height: 100rpx;
  bottom: 200rpx;
  left: 15%;
  background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="50" cy="50" r="40" fill="%23e9b872"/><circle cx="40" cy="40" r="5" fill="%23fff"/><circle cx="40" cy="40" r="2" fill="%23000"/><path d="M60 60 Q70 70 80 60" stroke="%23c48b44" stroke-width="3" fill="none"/><path d="M20 30 L10 20 M20 40 L5 40 M20 50 L10 60" stroke="%23c48b44" stroke-width="3" fill="none"/></svg>');
  animation-delay: -2s;
}

.fish {
  width: 120rpx;
  height: 80rpx;
  bottom: 300rpx;
  right: 15%;
  background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 50"><path d="M10,25 Q40,10 70,25 T95,25 L70,40 Q40,55 10,40 Z" fill="%238bbdd8"/><circle cx="20" cy="25" r="3" fill="%23000"/><path d="M80 25 L95 15 M80 25 L95 35" stroke="%238bbdd8" stroke-width="3"/></svg>');
  animation-delay: -1s;
  transform: scaleX(-1);
}

.pork {
  width: 110rpx;
  height: 90rpx;
  bottom: 440rpx;
  left: 70%;
  background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 80"><rect x="10" y="20" width="80" height="40" rx="20" fill="%23e4a199"/><circle cx="25" cy="35" r="5" fill="%23c97d76"/><circle cx="75" cy="35" r="5" fill="%23c97d76"/><path d="M40 50 Q50 55 60 50" stroke="%23c97d76" stroke-width="3" fill="none"/></svg>');
  animation-delay: -3s;
}

.duck {
  width: 100rpx;
  height: 100rpx;
  bottom: 100rpx;
  left: 45%;
  background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><ellipse cx="50" cy="60" rx="35" ry="30" fill="%23f0bc6b"/><circle cx="80" cy="40" r="15" fill="%23f0bc6b"/><path d="M85 35 L95 25" stroke="%23e0983b" stroke-width="3"/><circle cx="85" cy="35" r="3" fill="%23000"/></svg>');
  animation-delay: -4s;
}

.food-bowl {
  width: 140rpx;
  height: 100rpx;
  bottom: 360rpx;
  left: 35%;
  background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 60"><path d="M20,20 Q50,0 80,20 L80,40 Q50,60 20,40 Z" fill="%23e8e8e8"/><path d="M35,20 Q50,30 65,20" stroke="%23e63946" stroke-width="3" fill="none"/><path d="M35,25 Q50,35 65,25" stroke="%23e09f3e" stroke-width="3" fill="none"/><path d="M35,30 Q50,40 65,30" stroke="%239b2226" stroke-width="3" fill="none"/></svg>');
  animation-delay: -2.5s;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-30rpx);
  }
}

/* 山丘 */
.food-mountains {
  position: absolute;
  width: 100%;
  height: 40%;
  overflow: hidden;
  bottom: 0;
  z-index: 1;
}

.mountain {
  position: absolute;
  bottom: 0;
  width: 200rpx;
  height: 120rpx;
  background-color: #ffcc91;
  border-radius: 50% 50% 0 0;
  
  &::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 50% 50% 0 0;
    background: linear-gradient(135deg, rgba(255,255,255,0.2) 0%, rgba(0,0,0,0) 50%);
  }
}

.mountain-1 {
  left: 5%;
  height: 90rpx;
  opacity: 0.8;
}

.mountain-2 {
  left: 35%;
  height: 140rpx;
}

.mountain-3 {
  right: 5%;
  height: 110rpx;
  opacity: 0.9;
}

/* 头部标题 */
.header {
  position: relative;
  z-index: 1;
  flex-shrink: 0;
}

.title-wrapper {
  padding-top: 60rpx;
  position: relative;
  text-align: center;
  margin-bottom: 40rpx;
  z-index: 2;
  height: 400rpx; /* 明确的高度，确保标题区域有足够空间 */
}

.title {
  font-size: 56rpx;
  color: #8b4513;
  font-weight: bold;
  text-shadow: 0 2rpx 6rpx rgba(0,0,0,0.1);
  writing-mode: vertical-rl;
  position: absolute;
  top: 50rpx;
  left: 40rpx;
  height: 300rpx;
  letter-spacing: 8rpx;
  font-family: "KaiTi", "STKaiti", serif;
  z-index: 3;
}

.subtitle {
  font-size: 28rpx;
  color: #733a08;
  position: absolute;
  bottom: 20rpx;
  left: 50%;
  transform: translateX(-50%);
  background-color: rgba(255, 255, 255, 0.7);
  display: inline-block;
  padding: 6rpx 20rpx;
  border-radius: 20rpx;
  text-shadow: 0 2rpx 2rpx rgba(255, 255, 255, 0.8);
}

.logo-stamp {
  position: absolute;
  top: 50rpx;
  right: 40rpx;
  width: 130rpx;
  height: 130rpx;
  background-color: #8b4513;
  border-radius: 10rpx;
  display: flex;
  justify-content: center;
  align-items: center;
  transform: rotate(5deg);
  box-shadow: 0 6rpx 12rpx rgba(0,0,0,0.2);
  z-index: 3;
  
  &::before {
    content: '美食';
    color: #fff;
    font-family: "KaiTi", "STKaiti", serif;
    font-size: 44rpx;
    font-weight: bold;
    writing-mode: vertical-rl;
  }
}

/* 登录表单 */
.login-form {
  background: rgba(255, 247, 232, 0.85);
  padding: 50rpx;
  border-radius: 30rpx;
  box-shadow: 0 10rpx 40rpx rgba(0, 0, 0, 0.15);
  border: 2rpx solid rgba(212, 115, 63, 0.3);
  backdrop-filter: blur(8rpx);
  -webkit-backdrop-filter: blur(8rpx);
  margin-top: 20rpx;
  position: relative;
  z-index: 3;
  flex-shrink: 0;
  margin-bottom: 40rpx;
  
  &::before {
    content: '';
    position: absolute;
    top: -6rpx;
    left: -6rpx;
    right: -6rpx;
    bottom: -6rpx;
    background: linear-gradient(45deg, rgba(212, 115, 63, 0.2), rgba(255, 204, 145, 0.2), rgba(212, 115, 63, 0.2));
    border-radius: 32rpx;
    z-index: -1;
  }
}

.form-group {
  margin-bottom: 40rpx;
  position: relative;
}

.form-input {
  width: 100%;
  padding: 24rpx 30rpx;
  border: 2rpx solid rgba(212, 115, 63, 0.4);
  border-radius: 16rpx;
  background: rgba(255, 255, 255, 0.8);
  font-size: 28rpx;
  transition: all 0.3s;
  height: 100rpx;
  box-sizing: border-box;
  
  &:focus {
    outline: none;
    border-color: #8b4513;
    box-shadow: 0 0 0 4rpx rgba(139, 69, 19, 0.2);
    background: rgba(255, 255, 255, 0.95);
  }
}

.form-label {
  position: absolute;
  left: 30rpx;
  top: 24rpx;
  color: #8b4513;
  transition: all 0.3s;
  pointer-events: none;
  font-size: 28rpx;
  line-height: 1.5;
  
  &.active {
    top: -16rpx;
    left: 20rpx;
    font-size: 24rpx;
    background: #fff;
    padding: 0 10rpx;
    border-radius: 6rpx;
    box-shadow: 0 2rpx 6rpx rgba(0,0,0,0.05);
    line-height: 1.2;
  }
}

.btn-login {
  width: 100%;
  padding: 24rpx;
  background: #d4713f;
  color: white;
  border: none;
  border-radius: 16rpx;
  font-size: 30rpx;
  cursor: pointer;
  transition: all 0.3s;
  box-shadow: 0 4rpx 10rpx rgba(0,0,0,0.1);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 90rpx;
  box-sizing: border-box;
  
  &:active {
    transform: translateY(0);
    box-shadow: 0 4rpx 6rpx rgba(0,0,0,0.1);
  }
  
  &:not([disabled]):active {
    background: #a54b2a;
  }
  
  &.countdown-active,
  &[disabled] {
    background: #ccc !important;
    opacity: 0.7;
    color: #666 !important;
    pointer-events: none;
  }
}

.mt-15 {
  margin-top: 30rpx;
}

/* 响应式设计 */
@media screen and (max-width: 375px) {
  .title {
    font-size: 48rpx;
    height: 260rpx;
  }
  
  .logo-stamp {
    width: 110rpx;
    height: 110rpx;
  }
  
  .subtitle {
    font-size: 24rpx;
  }
  
  .login-form {
    padding: 40rpx;
  }
  
  .form-input {
    height: 90rpx;
    font-size: 26rpx;
  }
  
  .btn-login {
    height: 80rpx;
    font-size: 28rpx;
  }
}

@media screen and (min-width: 768px) {
  .title {
    font-size: 64rpx;
    height: 340rpx;
  }
  
  .logo-stamp {
    width: 150rpx;
    height: 150rpx;
  }
  
  .subtitle {
    font-size: 32rpx;
  }
  
  .login-form {
    padding: 60rpx;
  }
  
  .form-input {
    height: 110rpx;
    font-size: 30rpx;
  }
  
  .btn-login {
    height: 100rpx;
    font-size: 32rpx;
  }
}
</style> 