<template>
  <header>
    <div class="left">
      <div class="logo__wrapper">
        <span>Placeto</span>
      </div>
    </div>

    <div class="right">
      <p>Tutorial</p>
    </div>
  </header>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
header {
  padding-left: 15px;
  padding-right: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  height: 80px;
  background-color: white;
  box-shadow: 0 6px 6px 0 rgba(0, 0, 0, 0.1);

  .left {
    .logo__wrapper {
      span {
        font-weight: 700;
        font-size: 22px;
      }
    }
  }

  .right {
    p {
      font-size: 12px;
      color: black;
    }
  }
}
</style>
