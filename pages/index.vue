<template>
  <div class="container wrapper">
    <div class="row">
      <div class="col-12">
        <div class="status__wrapper">
          <div class="seg upload__wrapper">
            <label>
              <input type="file" name="file">
              <div class="upload__box">
                <div class="inner__box">
                  <span>Upload</span>
                </div>
              </div>
            </label>
          </div>

          <div class="seg image_preview__wrapper">
            <div class="image_ratio">
              <img src="" alt="Image preview">
            </div>
          </div>

          <div class="seg color__wrapper">
            <div class="color__bar">
              <div class="color__progress"/>
            </div>
          </div>

          <div class="seg object__wrapper">
            <div class="object_box">
              <span>general</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <article class="row">
      <div class="col-12 col-md-6 col-lg-3">
      </div>
    </article>
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
.wrapper {
  padding-top: 32px;
  padding-bottom: 32px;

  .status__wrapper {
    display: flex;
    justify-content: flex-start;
    margin-bottom: 22px;
    padding: 22px;
    background-color: white;
    box-shadow: 0 4px 4px 0 rgba(0, 0, 0, 0.08);

    .seg {
      padding-left: 22px;
      padding-right: 22px;
      border-right: 1px solid #eeeeee;
    }

    .seg:first-child {
      padding-left: 0;
    }

    .seg:last-child {
      padding-right: 0;
      border-right: 0;
    }

    .upload__wrapper {
      width: fit-content;

      label {
        cursor: pointer;

        input {
          display: none;
        }

        .upload__box {
          padding: 6px;
          width: 100px;
          height: 100px;
          border-radius: 6px;
          border: 1px dashed #cccccc;

          .inner__box {
            display: flex;
            justify-content: center;
            align-items: center;
            width: 100%;
            height: 100%;
            border-radius: 6px;
            background-color: #eeeeee;

            span {
              font-size: 16px;
            }
          }
        }
      }
    }

    .image_preview__wrapper {
      width: 144px;

      .image_ratio {
        overflow: hidden;
        position: relative;
        padding-bottom: 100%;
        width: 100%;
        border-radius: 4px;
        background-color: #cccccc;

        img {
          position: absolute;
          width: 100%;
          height: 100%;
        }
      }
    }

    .color__wrapper {
      width: 320px;

      .color__bar {
        overflow: hidden;
        margin-bottom: 10px;
        width: 100%;
        height: 10px;
        border-radius: 5px;
        background-color: #eeeeee;

        .color__progress {
          width: 50%;
          height: 100%;
          background-color: red;
        }
      }

      .color__bar:last-child {
        margin-bottom: 0;
      }
    }

    .object__wrapper {
      overflow-x: hidden;
      overflow-y: scroll;
      display: flex;
      flex-wrap: wrap;
      justify-content: flex-start;
      width: calc(100% - (123px + 144px + 320px + 22px));
      max-height: 100px;

      .object_box {
        margin-bottom: 6px;
        margin-right: 6px;
        padding-left: 16px;
        padding-right: 16px;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 28px;
        border-radius: 15px;
        border: 1px solid #cccccc;

        span {
          font-size: 14px;
        }
      }
    }
  }
}
</style>
