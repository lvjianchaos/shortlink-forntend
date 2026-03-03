<template>
  <div class="recycle-wrap">
    <div class="recycle-card">
      <div class="icon">🗑️</div>
      <h3>回收站</h3>
      <p>已删除的短链会展示在这里，支持恢复或永久删除。</p>
    </div>
  </div>
</template>

<script setup>

</script>

<style lang="scss" scoped>
.recycle-wrap {
  height: 100%;
  padding: 16px;
  background: linear-gradient(145deg, #eef2f8 0%, #e9eef6 50%, #edf1f8 100%);
}

.recycle-card {
  height: 100%;
  border-radius: 20px;
  background: linear-gradient(145deg, #edf2f9 0%, #e8eef7 100%);
  box-shadow:
    12px 12px 24px rgba(154, 171, 197, 0.34),
    -12px -12px 24px rgba(255, 255, 255, 0.95),
    inset 1px 1px 0 rgba(255, 255, 255, 0.82);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: #405f8f;

  h3 {
    margin: 10px 0 8px;
    font-size: 20px;
  }

  p {
    margin: 0;
    font-size: 13px;
    color: #6d83a6;
  }
}

.icon {
  width: 62px;
  height: 62px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 28px;
  background: linear-gradient(145deg, #ecf1f9 0%, #e4ebf5 100%);
  box-shadow:
    8px 8px 16px rgba(158, 176, 200, 0.3),
    -8px -8px 16px rgba(255, 255, 255, 0.92);
}

</style>