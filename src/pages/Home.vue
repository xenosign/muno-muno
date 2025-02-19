<template>
  <div class="homepage">
    <!-- Status Bar -->
    <div class="status-bar">
      <img
        src="@/assets/statusBar.svg"
        alt="Status Bar"
        class="status-bar-svg"
      />
    </div>

    <!-- 헤더 -->
    <div class="header">
      <img src="@/assets/munoLogo.svg" alt="Muno 로고" class="logo" />

      <router-link to="/mypage" class="mypage">
        <img
          src="@/assets/mypage.svg"
          alt="마이페이지 아이콘"
          class="mypage-icon"
        />
      </router-link>
    </div>

    <!-- 문어 레벨 섹션 -->
    <OctopusLevel
      :currentLevel="currentLevel"
      :currentOctopus="currentOctopus"
      :levelText="levelText"
      @navigate="navigateToProductRecommendation"
    />

    <!-- 문어 선택 섹션 -->
    <OctopusSelection :octopuses="octopuses" @select-octopus="selectOctopus" />

    <!-- 지출 금액 섹션 -->
    <ExpenseContainer :expenseAmount="expenseAmount" />

    <!-- TabBar 컴포넌트 사용 -->
    <TabBar />
  </div>
</template>

<script>
import ExpenseContainer from '@/components/ExpenseContainer.vue';
import OctopusLevel from '@/components/OctopusLevel.vue';
import OctopusSelection from '@/components/OctopusSelection.vue';
import TabBar from '@/components/TabBar.vue';

export default {
  name: 'HomePage',
  components: {
    ExpenseContainer,
    OctopusLevel,
    OctopusSelection,
    TabBar,
  },
  data() {
    return {
      expenseAmount: 100000,
      currentLevel: 1,
      levelText: '',
      currentOctopus: {},
      octopuses: [
        {
          id: 1,
          name: '예적금',
          levelImages: [
            require('@/assets/octopus_a1.svg'),
            require('@/assets/octopus_a2.svg'),
            require('@/assets/octopus_a3.svg'),
          ],
          buttonImage: require('@/assets/button_a.svg'),
        },
        {
          id: 2,
          name: '주식',
          levelImages: [
            require('@/assets/octopus_b1.svg'),
            require('@/assets/octopus_b2.svg'),
            require('@/assets/octopus_b3.svg'),
          ],
          buttonImage: require('@/assets/button_b.svg'),
        },
        {
          id: 3,
          name: '펀드',
          levelImages: [
            require('@/assets/octopus_c1.svg'),
            require('@/assets/octopus_c2.svg'),
            require('@/assets/octopus_c3.svg'),
          ],
          buttonImage: require('@/assets/button_c.svg'),
        },
        {
          id: 4,
          name: '채권',
          levelImages: [
            require('@/assets/octopus_d1.svg'),
            require('@/assets/octopus_d2.svg'),
            require('@/assets/octopus_d3.svg'),
          ],
          buttonImage: require('@/assets/button_d.svg'),
        },
      ],
    };
  },
  methods: {
    selectOctopus(octopus) {
      this.currentOctopus = {
        name: octopus.name,
        image: octopus.levelImages[this.currentLevel - 1],
      };
      this.levelText = `Lv.${this.currentLevel} ${octopus.name}`;
    },
    navigateToProductRecommendation() {
      this.$router.push('/product');
    },
    updateOctopusLevel(newLevel) {
      this.currentLevel = newLevel;
      this.currentOctopus.image =
        this.currentOctopus.levelImages[this.currentLevel - 1];
      this.levelText = `Lv.${this.currentLevel} ${this.currentOctopus.name}`;
    },
    fetchLevelFromBackend() {
      fetch('/api/octopus/level')
        .then((response) => response.json())
        .then((data) => {
          this.updateOctopusLevel(data.level);
        })
        .catch((error) => {
          console.error('Error fetching level from backend:', error);
        });
    },
    setInitialOctopus() {
      const initialOctopus = this.octopuses[0];
      this.selectOctopus(initialOctopus);
    },
  },
  mounted() {
    this.fetchLevelFromBackend();
    this.setInitialOctopus();
  },
};
</script>

<style scoped>
.homepage {
  background-color: var(--p70);
  overflow-y: hidden;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100vh;
}

/* Status Bar*/
.status-bar {
  background-color: var(--p70);
  height: 44px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 1000;
}

.status-bar-svg {
  width: 375px;
  height: 44px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 22px;
  height: 65px;
  background-color: var(--p70);
  position: fixed;
  top: 44px;
  left: 0;
  width: 100%;
  z-index: 1000;
  box-sizing: border-box;
}

.mypage-icon {
  width: 24px;
  height: 24px;
}
</style>
