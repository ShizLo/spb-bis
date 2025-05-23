<script setup>
import Swiper from "swiper/bundle";
import "swiper/css/bundle";
import { Navigation, Autoplay } from "swiper/modules";
import "../assets/styles/banner/banner.scss";
import guaranteeIcon from "../assets/icons/Icons_BIS/SVG/guarantee.svg";
import serviceIcon from "../assets/icons/Icons_BIS/SVG/montag+.svg";
import keyIcon from "../assets/icons/Icons_BIS/SVG/workKey.svg";
import { useDisplay } from "vuetify";

const router = useRouter();

const { mobile } = useDisplay();
const props = defineProps({
  bannerText: {
    typeof: String,
  },
  bannerDescription: {
    typeof: String,
  },
  desctiption: {
    typeof: Array,
  },
  bannerPrice: {
    typeof: Array,
  },
  stikers: {
    typeof: Boolean,
  },
});

const swiperBanner = reactive({
  slideIndex: 0, // Индекс текущего слайда
});

const swiper_banner_setting = reactive({
  instance: null,
  settings: {
    direction: "horizontal",
    slidesPerView: 1, // колво слайдов на показ
    loop: true, // бесконечный слайдер
    speed: 2500,
    parallax: true,
    effect: "fade",
    fadeEffect: {
      crossFade: true,
    },
    autoplay: {
      delay: 3000,
      // Отключить после ручного переключения
      disableOnInteraction: true,
      // Остановится на последнем слайде
      stopOnLastSlide: false,
    },
    pagination: {
      el: ".swiper-pagination",
      type: "bullets",
      clickable: true,
    },
    navigation: {
      nextEl: ".swiper-button-next",
      prevEl: ".swiper-button-prev",
    },
    on: {
      slideChange: (swiper) => {
        swiperBanner.slideIndex = swiper.realIndex;
      },
    },
  },
  modules: [Navigation, Autoplay],
});

onMounted(() => {
  swiper_banner_setting.instance = new Swiper(".swiper-banner", swiper_banner_setting.settings);
});

onUnmounted(() => {
  if (swiper_banner_setting.instance) {
    swiper_banner_setting.instance.destroy();
  }
});
function hoverMobile() {
  // document.querySelectorAll(".banner__button").forEach((element) => {
  //   element.addEventListener("touchstart", () => element.classList.add("on-hover"));
  //   element.addEventListener("touchend", () => element.classList.remove("on-hover"));
  // });
}

function goTo(path) {
  router.push(path);
}
</script>

<template>
  <section class="banner">
    <div class="banner__container _container">
      <div class="banner__block-text text-left">
        <h1 class="banner__text _visibility-mobile">{{ bannerText }}</h1>
        <h1 v-if="desctiption" class="banner__text _visibility">{{ desctiption[swiperBanner.slideIndex].title }}</h1>
        <h1 v-else-if="bannerText.length > 0" class="banner__text _visibility">{{ bannerText }}</h1>
        <div v-if="stikers && !mobile">
          <div class="d-flex align-center">
            <v-icon size="34" class="mr-1">
              <v-img :src="guaranteeIcon"></v-img>
            </v-icon>
            <div>Гарантия на все работы</div>
          </div>
          <div class="d-flex align-center">
            <v-icon size="34" class="mr-1">
              <v-img :src="serviceIcon"></v-img>
            </v-icon>
            <div>Своя сервисная служба</div>
          </div>
          <div class="d-flex align-center">
            <v-icon size="34" class="mr-1">
              <v-img :src="keyIcon"></v-img>
            </v-icon>
            <div>Работа под ключ</div>
          </div>
        </div>
        <!-- <button class="banner__button" @click="hoverMobile(), $emit('isVisible')">Обсудить задачу</button> -->
        <v-btn
          @click="hoverMobile(), $emit('isVisible')"
          type="submit"
          :size="mobile ? 'large' : 'x-large'"
          class="submit-btn"
          :append-icon="mobile ? '' : 'mdi-arrow-right'"
          variant="flat"
          color="orange-darken-2"
        >
          Обсудить задачу
        </v-btn>
        <p class="banner__description _visibility-mobile">{{ bannerDescription }}</p>
        <p v-if="desctiption" class="banner__description _visibility">{{ desctiption[swiperBanner.slideIndex].text }}</p>
        <h1 v-else-if="bannerDescription" class="banner__description _visibility">{{ bannerDescription }}</h1>
      </div>
      <div class="swiper-container text-left">
        <div class="swiper-banner">
          <div class="swiper-wrapper">
            <div v-for="item in bannerPrice" class="swiper-slide">
              <!-- <img class="swiper__img" loading="lazy" :src="item.url" :alt="item.name" /> -->
              <NuxtImg class="swiper__img" :src="item.url" :alt="item.name" loading="lazy" />
              <div class="swiper-lazy-preloader-white"></div>
              <div @click="goTo(item.routerPath)" class="swiper-banner__content">
                <div v-if="item.price.length > 0" class="content__price">
                  <span>{{ item.price }}</span>
                </div>
                <div class="content__title">{{ item.name }}</div>
              </div>
            </div>
          </div>
          <div class="swiper-pagination"></div>
          <div class="swiper-button-prev"></div>
          <div class="swiper-button-next"></div>
        </div>
      </div>
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use "../assets/styles/main.scss" as *;

@media (max-width: $md3) {
  ._container {
    padding: 0;
  }
}

._visibility {
  display: none;
  @media (min-width: $md1) {
    display: block;
  }
}
._visibility-mobile {
  display: none;
  @media (max-width: $md4) {
    display: block;
  }
}

.swiper-container {
  width: 100%;
  height: 360px;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 1px 1px 3px 0px rgba(34, 60, 80, 0.25);
  @media (max-width: $md2) {
    border-radius: 0;
    // &::after {
    //   content: "";
    //   width: 100%;
    //   height: 2px;
    //   position: absolute;
    //   background-color: #ea5b0c;
    // }
  }
  @media (max-width: $md4) {
    height: 300px;
  }
}
.swiper-banner {
  position: relative;
  max-width: 100%;
  height: 100%;
  margin: auto;
  margin: auto;
  &__content {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    top: 0;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: flex-start;
    padding: 30px 30px 22px;
    @media (max-width: $md3) {
      padding: 25px 10px 22px;
    }
    @media (max-width: $md4) {
      padding: 25px 14px 18px;
    }
  }
}
.swiper-slide {
  position: relative;
}
.swiper__img {
  height: 100%;
  width: 100%;
  object-fit: cover;
}
.swiper-banner__content::before {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 25%;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0));
  z-index: 2;
}
.content__price {
  font-size: 1rem;
  font-weight: 600;
  background: #fff;
  display: inline-block;
  border-radius: 8px;
  padding: 3px 10px;
  color: #102938;
  border: 0.5px solid #ea5b0c;
  box-shadow: 1px 1px 4px 0px rgba(34, 60, 80, 0.18);
  @media (max-width: $md4) {
    font-size: 14px;
  }
}
.swiper-banner .content__title {
  z-index: 3;
  font-weight: 600;
  font-size: 1.6rem;
  color: #fff;
  width: calc(100% - 68px);
  line-height: normal;
  @media (max-width: $md4) {
    font-size: 18px;
  }
}
/* Кнопки */
.swiper-banner .swiper-button-prev {
  color: #fff;
  z-index: 2;
  @media (max-width: $md2) {
    display: none;
  }
}
.swiper-banner .swiper-button-next {
  color: #fff;
  z-index: 2;
  @media (max-width: $md2) {
    display: none;
  }
}
/* Буллеты */
.swiper-banner .swiper-pagination {
  bottom: 0;
  z-index: 2;
}
.swiper-banner .swiper-pagination-bullet {
  width: 30px;
  border-radius: 8px;
  height: 4px;
  background-color: #ea5b0c;
}
.swiper-banner .swiper-pagination-bullet-active {
  position: relative;
  width: 60px;
  border-radius: 8px;
  height: 4px;
  /* opacity: 0.2; */
  // z-index: 11;
  // background-color: #ea5b0c;
}
.swiper-banner .swiper-pagination-bullet-active::after {
  content: "";
  position: absolute;
  // z-index: 12;
  top: 0;
  left: 0;
  display: flex;
  flex-direction: column;
  /* width: 100%; */
  height: 100%;
  // width: 0;
  opacity: 1;

  // #ea5b0c

  transition: background-color 0.4s linear;

  animation: progress-css 1s ease-in forwards;
}

.swiper-banner .swiper-pagination-bullet-active::after {
  /* Включает анимацию булитов */
  width: 60px;
  background-color: red;
  transition: background-color 4s linear;
  transition-property: width, background-color;
}

//@use "../assets/styles/app.scss" as c;

@media (min-width: $md1) {
  .banner__button:hover {
    background: #102938;
    color: #f5f5f5;
    border-color: #102938;
  }
}
.on-hover:hover {
  background: #102938;
  transition: 10ms;
  color: #f5f5f5;
  border-color: #102938;
}

@media (max-width: $md3) {
  ._container {
    padding: 0;
  }
}
.banner-gallery__elems {
  position: absolute;
  z-index: 3;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 25px;
  padding: 0 30px 30px;
}

.banner {
  background-color: $color-background;
  margin: 0px 0px 25px 0px;
  @media (max-width: $md4) {
    background-color: #fff;
    margin: 0px 0px 0px 0px;
  }
}
.banner {
  @media (min-width: $md2) {
    // &::after {
    //   content: "";
    //   width: 100%;
    //   height: 2px;
    //   position: absolute;
    //   background-color: #ea5b0c;
    // }
  }

  &__container {
    display: flex;
    // padding: 20px 0px 20px 0px;
    padding-top: 20px;
    padding-bottom: 20px;
    @media (max-width: $md2) {
      flex-wrap: wrap-reverse;
      padding: 0px 0px 0px 0px;
    }
  }

  &__block-text {
    display: flex;
    width: 100%;
    flex-direction: column;
    // justify-content: center;
    // justify-content: space-between;
    align-items: flex-start;
    gap: 25px;

    @media (max-width: $md2) {
      padding: 20px 10px;
      gap: 15px;
    }
    @media (max-width: $md4) {
      align-items: center;
      padding-bottom: 10px;
      gap: 10px;
    }
  }
  &__text {
    font-weight: 600;
    line-height: 120%;
    font-size: 2rem; //40
    color: #102938;
    @media (min-width: $md1) {
      margin-top: 15px;
    }
    @media (max-width: $md2) {
      font-size: 1.5rem;
    }
    @media (max-width: $md4) {
      font-size: 26px;
      // text-align: center;
      // align-self: center;
    }
  }
  &__description {
    padding-right: 15px;
    font-size: inherit;
    font-weight: inherit;

    @media (max-width: $md4) {
      line-height: normal;
      font-size: 13px;
    }
  }
  &__button {
    font-weight: 500;
    display: flex;
    height: 48px;
    padding: 11px 14px;
    align-items: center;
    gap: 8px;
    border-radius: 4px;
    border: 1px solid #ea5b0c;
    box-shadow: 1px 1px 1px 0px rgba(34, 60, 80, 0.18);
    background: rgba(245, 245, 245, 0);
    color: #102938;
    text-align: center;
    cursor: pointer;
    transition: all 0.3s ease 0s;
    @media (max-width: $md3) {
      align-self: flex-end;
    }
  }
  &__button-link {
    @media (max-width: $md4) {
      align-self: end;
    }
  }
}
/* Буллеты */
.swiper-banner .swiper-pagination {
  bottom: 0;
}
.swiper-banner .swiper-pagination-bullet {
  width: 30px;
  border-radius: 8px;
  height: 4px;
  background-color: #fff;
}
.swiper-banner .swiper-pagination-bullet-active {
  position: relative;
  width: 60px;
  border-radius: 8px;
  height: 4px;
}
.swiper-banner .swiper-pagination-bullet-active::after {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  flex-direction: column;
  height: 100%;
  width: 0;
  opacity: 1;
  border-radius: 8px;
  transition: background-color 0.4s linear;
  animation: progress-css 1s ease-in forwards;
}

.swiper-banner .swiper-pagination-bullet-active::after {
  /* Включает анимацию булитов */
  width: 100%;
  background-color: #ea5b0c;
  transition: all 4s linear;
  transition-property: width, background-color;
}
.submit-btn {
  background: linear-gradient(90deg, #ea5b0c, #ff8c42);
  letter-spacing: 0.5px;
  font-weight: 600;
  text-transform: uppercase;
  transition: all 0.3s ease;
  box-shadow: 0 4px 6px rgba(234, 91, 12, 0.1);

  &:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 12px rgba(234, 91, 12, 0.2);
  }
}
</style>
