<template>
  <div class="mypage">
    <HeaderB title="마이페이지" @back="goBack" />

    <!-- 프로필 섹션 -->
    <div class="profile-section">
      <div class="profile-image-container">
        <img
          src="@/assets/profile.svg"
          alt="프로필 이미지"
          class="profile-image"
        />
        <button class="edit-icon">
          <img src="@/assets/edit.svg" alt="프로필 수정" />
        </button>
      </div>

      <!-- 사용자 정보 섹션 -->
      <div class="user-info">
        <div class="user-detail">
          <span class="user-q">닉네임</span>
          <span class="user-a">{{
            user.nickname || '닉네임을 등록해주세요'
          }}</span>
        </div>
        <div class="user-detail">
          <span class="user-q">생년월일</span>
          <span class="user-a">{{
            user.birthDate || '생년월일을 등록해주세요'
          }}</span>
        </div>
        <div class="user-detail">
          <span class="user-q">휴대폰 번호</span>
          <span class="user-a">{{
            user.phoneNumber || '휴대폰 번호를 등록해주세요'
          }}</span>
        </div>
        <div class="user-detail">
          <span class="user-q">이메일 주소</span>
          <span class="user-a">{{
            user.email || '이메일 주소를 등록해주세요'
          }}</span>
        </div>
      </div>
    </div>

    <!-- 프로필 수정 버튼 -->
    <ButtonA class="profile-button" @click="goToEditPage">프로필 수정</ButtonA>
  </div>
</template>

<script>
import HeaderB from '@/components/HeaderB.vue';
import ButtonA from '@/components/ButtonA.vue';
import axios from 'axios';

export default {
  name: 'MyPage',
  components: {
    HeaderB,
    ButtonA,
  },
  data() {
    return {
      user: {
        nickname: '',
        birthDate: '',
        phoneNumber: '',
        email: '',
      },
    };
  },
  methods: {
    goBack() {
      this.$router.push('/');
    },
    // 프로필 수정 페이지로 이동
    goToEditPage() {
      this.$router.push({
        name: 'MyPageEdit',
        params: { user: this.user }, // user 데이터를 params로 전달
      });
    },
    fetchUserProfile() {
      axios
        .get('/api/user/profile')
        .then((response) => {
          this.user = response.data;
        })
        .catch((error) => {
          console.error('Error fetching user profile:', error);
        });
    },
  },
  mounted() {
    this.fetchUserProfile();
  },
};
</script>

<style scoped>
.mypage {
  padding: 0 25px;
  background-color: var(--gr100);
  height: 100vh;
}

.profile-section {
  margin-top: 84px;
}

.profile-image-container {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

.profile-image {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  margin-bottom: 40px;
}

.edit-icon {
  position: absolute;
  bottom: 0;
  right: 0;
  background-color: var(--gr100);
  border: none;
  cursor: pointer;
}

.user-info {
  margin-top: 20px;
}

.user-detail {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
}

.user-q {
  color: var(--gr20);
  font-size: 16px;
  font-weight: 600;
}

.user-a {
  color: var(--gr50);
  font-size: 16px;
  font-weight: 400;
  text-align: right;
}

.profile-button {
  position: fixed;
  bottom: 34px;
  left: 50%;
  transform: translateX(-50%);
  width: calc(100% - 40px);
  background-color: var(--p10);
  color: var(--gr100);
  font-size: 16px;
  padding: 10px;
  border: none;
  cursor: pointer;
  border-radius: 8px;
  text-align: center;
}
</style>
