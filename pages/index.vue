<template>
  <div class="container wrapper">
    <div class="row">
      <div class="col-12">
        <div class="status__wrapper">
          <div class="seg upload__wrapper">
            <label>
              <input ref="file" type="file" name="file" @change="changeImageUpload">
              <div class="upload__box">
                <div class="inner__box">
                  <span>Upload</span>
                </div>
              </div>
            </label>
          </div>

          <div v-show="imageBase64 !== ''" class="seg image_preview__wrapper">
            <div class="image_ratio">
              <img :src="imageBase64" alt="Image preview">
            </div>
          </div>

          <div v-show="clarifai.complate" class="seg color__wrapper">
            <div
              v-for="(color, colorIndex) in clarifai.colors.slice(0, 5)"
              :key="colorIndex"
              class="color__bar"
            >
              <div
                class="color__progress"
                :style="{
                  width: `${color.value}%`,
                  backgroundColor: color.raw_hex
                }"
              />
            </div>
          </div>

          <div v-show="clarifai.complate" class="seg object__wrapper">
            <div
              v-for="(general, objIndex) in clarifai.generals"
              :key="objIndex"
              class="object_box"
            >
              <span>{{ general.name }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <article class="row">
      <div
        v-for="place in listPlaces"
        :key="place.id"
        class="col-12 col-md-6 col-lg-3"
      >
        <Card
          :name="place.name"
          :price="place.price"
          :province="place.province"
          :image-path="place.image_path"
          :objects="place.objects"
        />
      </div>
    </article>
  </div>
</template>

<script>
import placeJson from '@/data/places.json'

const card = () => import('@/components/ui/card')

export default {
  components: {
    Card: card
  },

  data () {
    return {
      listPlaces: [],
      imageBase64: '',
      clarifai: {
        isLoading: false,
        complate: false,
        generals: [],
        colors: []
      }
    }
  },

  watch: {
    imageBase64 () {
      this.clarifai = {
        isLoading: false,
        complate: false,
        colors: [],
        generals: []
      }

      this.toGeneralClarifai()
      this.toColorClarifai()
    },

    'clarifai.generals' (generals) {
      if (generals.length > 0) {
        this.checkComplateClarifai()
      }
    },

    'clarifai.colors' (colors) {
      if (colors.length > 0) {
        this.checkComplateClarifai()
      }
    }
  },

  created () {
    this.listPlaces = placeJson
  },

  methods: {
    getBase64 (file) {
      const reader = new FileReader()
      reader.readAsDataURL(file)
      reader.onload = () => {
        this.imageBase64 = reader.result
      }
      reader.onerror = (error) => {
        // eslint-disable-next-line
        console.log(`Error: ${error}`)
      }
    },

    changeImageUpload (e) {
      const file = this.$refs.file.files[0]

      if (file) {
        this.getBase64(file)
      }
    },

    toColorClarifai () {
      const base64 = this.imageBase64.split(',')[1]

      fetch(`${process.env.CLARIFAI_COLOR_MODEL_URL}`, {
        method: 'POST',
        headers: {
          Authorization: `Key ${process.env.CLARIFAI_API_KEY}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          inputs: [
            {
              data: {
                image: {
                  base64
                }
              }
            }
          ]
        })
      })
        .then(res => res.json())
        .then((data) => {
          const colors = data.outputs[0].data.colors

          const arr = []

          colors.forEach((color) => {
            arr.push(color.w3c.name)

            this.clarifai.colors.push({
              raw_hex: color.raw_hex,
              value: color.value * 100,
              w3c_name: color.w3c.name
            })
          })

          console.log(arr)
        })
        .catch((err) => {
          // eslint-disable-next-line
          console.log(err)
        })
    },

    toGeneralClarifai () {
      const base64 = this.imageBase64.split(',')[1]

      fetch(`${process.env.CLARIFAI_GENERAL_MODEL_URL}`, {
        method: 'POST',
        headers: {
          Authorization: `Key ${process.env.CLARIFAI_API_KEY}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          inputs: [
            {
              data: {
                image: {
                  base64
                }
              }
            }
          ]
        })
      })
        .then(res => res.json())
        .then((data) => {
          const concepts = data.outputs[0].data.concepts

          const arr = []

          concepts.forEach((concept) => {
            arr.push(concept.name)
            this.clarifai.generals.push({
              name: concept.name,
              value: concept.value * 100
            })
          })

          console.log(arr)
        })
        .catch((err) => {
          // eslint-disable-next-line
          console.log(err)
        })
    },

    checkComplateClarifai () {
      if (this.clarifai.colors.length > 0 && this.clarifai.generals.length > 0) {
        this.filterPlace()

        this.clarifai.complate = true
      }
    },

    async filterPlace () {
      this.listPlaces = []

      const colors = await this.clarifai.colors
      const generals = await this.clarifai.generals

      const arrColorPlaces = await []
      const arrgeneralPlaces = await []

      await placeJson.forEach((place) => {
        place.colors.forEach((colorPlace) => {
          colors.forEach((colorClarifai) => {
            if (colorPlace.toLowerCase() === colorClarifai.w3c_name.toLowerCase()) {
              arrColorPlaces.push(place)
            }
          })
        })

        place.objects.forEach((colorPlace) => {
          generals.forEach((generalClarifai) => {
            if (colorPlace.toLowerCase() === generalClarifai.name.toLowerCase()) {
              arrgeneralPlaces.push(place)
            }
          })
        })
      })

      const mergeArrPlaces = await arrColorPlaces.concat(arrgeneralPlaces)
      const newPlaces = [...new Set(mergeArrPlaces)]

      this.listPlaces = newPlaces
    }
  }
}
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
