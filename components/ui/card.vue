<template>
  <section>
    <div class="image__frame">
      <img :src="require(`@/assets/images/${imagePath}`)" :alt="name">
    </div>

    <div class="content__wrapper">
      <div class="seg">
        <div class="tag__box">
          <a
            v-for="(object, objectIndex) in objects.slice(0, 3)"
            :key="objectIndex"
          >
            #{{ object }}
          </a>
        </div>
      </div>

      <div class="seg">
        <div class="left name_provice__box">
          <h3>{{ name }}</h3>
          <p>{{ province }}</p>
        </div>
        <div class="right price__box">
          <p>฿{{ price }}</p>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      required: true
    },
    price: {
      type: Number,
      required: true
    },
    province: {
      type: String,
      required: true
    },
    imagePath: {
      type: String,
      required: true
    },
    objects: {
      type: Array,
      required: true
    }
  }
}
</script>

<style lang="scss" scoped>
section {
  transition: 200ms ease;
  cursor: pointer;
  margin-bottom: 30px;
  box-shadow: 0 12px 12px -4px rgba(0, 0, 0, 0.1);

  .image__frame {
    position: relative;
    width: 100%;
    padding-bottom: 100%;

    img {
      position: absolute;
      width: 100%;
      height: 100%;
      background-color: #cccccc;
    }
  }

  .content__wrapper {
    padding: 16px 12px;
    display: flex;
    flex-direction: column;

    .seg {
      display: flex;
      justify-content: space-between;
    }

    .tag__box {
      margin-bottom: 6px;
      overflow: hidden;
      display: flex;
      align-self: center;

      a {
        margin-right: 8px;
        white-space: nowrap;
        font-size: 12px;
        color: #3498DB;
      }
    }

    .name_provice__box {
      margin-right: 6px;
      width: calc(70% - 6px);

      h3 {
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        font-weight: 700;
        font-size: 18px;
      }

      p {
        white-space: nowrap;
        font-size: 12px;
        color: #333333;
      }
    }

    .price__box {
      margin-left: 6px;
      display: flex;
      align-items: center;
      width: calc(30% - 3px);

      p {
        text-align: right;
        font-weight: 700;
        font-size: 18px;
      }
    }
  }
}

section:hover {
  transform: translateY(-10px);
  box-shadow: 0 22px 22px -4px rgba(0, 0, 0, 0.08);
}
</style>
