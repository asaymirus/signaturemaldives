<template>
  <div class="header">
    <div class="header__content">
      <div class="header__brand">
        <h1 class="header__title">
          <span class="brand-icon">✨</span>
          ITD Signature Brand
        </h1>
        <p class="header__subtitle">Create Professional Email Signatures for Gmail</p>
      </div>
      <div class="header__actions">
        <el-button
          type="text"
          icon="el-icon-link"
          @click="openGitHub"
        >
          GitHub
        </el-button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Header',

  methods: {
    openGitHub () {
      window.open('https://github.com/antonreshetov/mysigmail', '_blank')
    }
  }
}
</script>

<style lang="scss">
.header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 20px 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);

  &__content {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  &__brand {
    flex: 1;
  }

  &__title {
    margin: 0;
    font-size: 28px;
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 10px;

    .brand-icon {
      font-size: 32px;
      display: inline-block;
    }
  }

  &__subtitle {
    margin: 5px 0 0 0;
    font-size: 14px;
    opacity: 0.9;
  }

  &__actions {
    button {
      color: white !important;

      &:hover {
        color: #f0f0f0 !important;
      }
    }
  }
}
</style>
