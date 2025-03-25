<template>
  <div>
    <button id="backButton" style="display:none;" @click="goBack">← 返回上一步</button>
    <div class="container">
      <h1>不要再问吃啥</h1>
      
      <div id="step1" class="step" :class="{ 'step-animation': activeStep === 1 }">
        <h2>STEP 1. 选择用餐时段</h2>
        <button @click="selectMeal('早餐')">🌅 早餐</button>
        <button @click="selectMeal('中午')">☀️ 午餐</button>
        <button @click="selectMeal('晚上')">🌙 晚餐</button>
        <button @click="selectMeal('夜宵')">🌃 夜宵</button>
      </div>

      <div id="step2" class="step" v-show="activeStep === 2">
        <h2>STEP 2. 选择想吃类型</h2>
        <div id="foodButtons" class="button-group"></div>
      </div>

      <div id="step3" class="step" v-show="activeStep === 3">
        <h2>STEP 3. 推荐优质店铺</h2>
        <div id="shopList"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeStep: 1,
      selectedMeal: null,
      selectedType: null,
      foodData: {
        早餐: {
          胡辣汤: ['星河葱花饼胡辣汤', '铺安街建宏胡辣汤', '东湖早市胡辣汤'],
          水煎包: ['老钱家水煎包', '禹都小刚锅贴'],
          葱花饼: ['闻喜葱花饼馍花馆', '黄河早市'],
          豆腐脑油条: ['明收源对面', '西花园对面', '早市'],
          饼子夹: ['随便路边吧没得推荐'],
          包子: ['瞪眼包', '铺安街小笼包', '王老二包子', '其他不知道了'],
          不吃: ['是不是起不来？']
        },
        中午: {
          大盘鸡: ['蛋蛋家老五炒鸡', '星河摇滚炒鸡', '腾龙大盘鸡总店', '随便就近一家鸡店'],
          面条: ['存才', '隆兴美', '酱香面馆', '裤带面', '兰州拉面', '平陆油泼面', '中银路小王鱼加面', '龙凤手擀面', '随便就近一家炒面'],
          羊肉泡: ['王剑羊肉泡', '颐祥阁水盆羊肉', '东郭胖子羊汤', '北相羊肉胡卜', '随便就近一家羊汤'],
          火锅干锅铜锅: ['清水阁', '一尊黄牛', '辣鸭头', '绛州铜火锅', '论斤的清水涮', '王婆大虾'],
          鱼: ['任老师', '二峰鱼馆'],
          米饭炒菜: ['豪德湘菜', '杨家附近湘菜', '冉师川菜', '九九九甘美', '阿宝川菜', '运中后门盖饭', '条山街蜀火爆江湖菜', '南山湘', '0359家常菜', '哈尔滨粥屋'],
          自助: ['潮汕牛肉自助', '李姑婆牛肉火锅', '深航', '寿喜烧', '九田家', '比格'],
          麻辣烫米线: ['随便就近吧'],
          铁锅炖: ['原王庄里', '就近一家'],
          汉堡披萨: ['你们不吃这个']
        },
        晚上: {
          铁锅炖: ['原王庄里', '就近一家'],
          涮锅: ['清水阁', '论斤的清水涮', '李姑婆牛肉火锅', '摸错门'],
          烧烤: ['老王', '烤大侠', '喜洋洋', '满堂红', '东盛羊蝎子烤羊腿'],
          自助: ['同中午的'],
          小吃: ['御沁园', '禹都小吃', '各种鸭货蛋蛋最爱']
        },
        夜宵: {
          喝酒: ['花你别乱点'],
          洗澡: ['杨你别乱点'],
          唱歌: ['钱哥你别乱点']
        }
      }
    }
  },
  methods: {
    selectMeal(mealType) {
      this.selectedMeal = mealType
      this.activeStep = 2
    },
    selectType(foodType) {
      this.selectedType = foodType
      this.activeStep = 3
    },
    goBack() {
      this.activeStep = Math.max(1, this.activeStep - 1)
      if (this.activeStep === 1) this.selectedMeal = null
    }
  }
}
</script>

<style>
/* 基础样式 */
:root {
  --gradient-start: #FF9A9E;
  --gradient-end: #FAD0C4;
  --primary-color: #FF6B6B;
  --secondary-color: #4ECDC4;
  --background-color: linear-gradient(135deg, var(--gradient-start) 0%, var(--gradient-end) 100%);
  --card-bg: rgba(255, 255, 255, 0.95);
  --text-color: #2D3436;
  --shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  --radius: 12px;
}

/* 现代视觉样式 */
.container {
  background: var(--background-color);
  min-height: 100vh;
  padding: 2rem;
}

button {
  background: var(--card-bg);
  border: 2px solid var(--primary-color);
  border-radius: var(--radius);
  padding: 1rem 2rem;
  margin: 0.8rem;
  box-shadow: var(--shadow);
  transition: all 0.3s ease;
}

button:hover {
  transform: translateY(-2px);
  box-shadow: 0 12px 24px rgba(255, 107, 107, 0.2);
}

.step {
  background: var(--card-bg);
  border-radius: var(--radius);
  padding: 2rem;
  margin: 2rem auto;
  max-width: 800px;
  box-shadow: var(--shadow);
}

/* 响应式布局调整 */
@media (max-width: 768px) {
  .container {
    padding: 1rem;
  }
  h1 {
    font-size: 1.8rem;
    text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
  }
  button {
    width: 100%;
    margin: 0.5rem 0;
    padding: 0.8rem;
  }
  .step {
    padding: 1.5rem;
    margin: 1rem 0;
  }
}

/* 步骤切换动画 */
.step-animation {
  animation: slideIn 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

button {
  position: relative;
  overflow: hidden;
}

button:active::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  padding-top: 0;
  border-radius: 50%;
  background: rgba(255,255,255,0.3);
  transform: translate(-50%, -50%);
  animation: ripple 0.4s ease-out;
}

@keyframes ripple {
  from {
    width: 0;
    padding-top: 0;
  }
  to {
    width: 150%;
    padding-top: 150%;
  }
}
</style>

@media (max-width: 400px) {
  button {
    margin: 0.3rem 0;
    padding: 0.6rem;
  }
  .step {
    padding: 1rem;
  }
}

#shopList {
  display: grid;
  gap: 1rem;
}

.shop-item {
  padding: 1.2rem;
  background: var(--card-bg);
  border-left: 4px solid var(--primary-color);
  transition: transform 0.3s ease;
}

.shop-item:hover {
  transform: translateX(8px);
}
</style>
