<template>
    <div class="basis-full md:basis-1/2 lg:basis-1/3 card" :class="{ 'unavailable': props.unavailable }">
      <div class="overlay" v-if="props.unavailable"></div>
      <div>
        <a :href="props.item.wikiUrl" target="_blank">{{ props.item.location.name }}</a>
        <img :src="props.item.domainPreviewImage" alt="">

        <div class="card--content">
          <div class="flex flex-row availability">
            <div class="basis-full">
              <span v-for="availability in props.item.location.availability">{{ availability }}</span>
            </div>
          </div>

          <div class="flex flex-row loot">
            <div class="basis-1/5" v-for="loot in props.item.location.materials">
              <div :class="rarityClassName(loot.rarity)" :title="loot.name">
                <img :src="loot.iconURL" alt="">
              </div>
            </div>
          </div>

          <div class="card--recommendations">
            <span class="card--recommendations--icon" v-for="element in props.item.recommendedElements">
              <div :class="element"></div>
            </span>
          </div>
        </div>

      </div>
    </div>
</template>




<script setup lang="ts">
  import { onMounted } from "vue";

  // Setting up props
  const props = defineProps<{
    item?: any
    unavailable?: any
  }>()



  // Setting up data properties - refs and reactives





  // Setting up lifecycle hooks
  onMounted(() => {
  })



  // Setting up computed properties




  // Setting up methods
  const rarityClassName = (rarity: any) => {
    return `loot--${rarity}-star`
  }




  // Setting up watches
</script>





<style scoped lang="scss">
  .card {
    position: relative;
    padding: 2em;

    &.unavailable {
      filter: grayscale(1);
      opacity: .5;

      .overlay {
        width: calc(100% - 60px);
        height: calc(100% - 65px);
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        left: 0;
        right: 0;
        z-index: 1111;
        margin-left: auto;
        margin-right: auto;
      }
    }

    &--content {
      padding: 15px;

      .availability {
        opacity: .6;
        margin-top: -45px;
        margin-bottom: 25px;

        span {
          &+span {
            margin-left: 5px;

            &::before {
              content: "/  ";
            }
          }
        }
      }
    }

    &--recommendations {
      display: flex;
      justify-self: end;
      justify-content: flex-end;
      margin-top: 15px;
      margin-bottom: -15px;
      padding: 9px;

      &--icon {
        >div {
          width: 25px;
          height: 25px;
          background-size: cover;

          &.pyro {
            background-image: url('@/assets/img/elements/pyro.png');
          }

          &.hydro {
            background-image: url('@/assets/img/elements/hydro.png');
          }

          &.dendro {
            background-image: url('@/assets/img/elements/dendro.png');
          }

          &.cryo {
            background-image: url('@/assets/img/elements/cryo.png');
          }

          &.electro {
            background-image: url('@/assets/img/elements/electro.png');
          }

          &.anemo {
            background-image: url('@/assets/img/elements/anemo.png');
          }

          &.geo {
            background-image: url('@/assets/img/elements/geo.png');
          }
        }
      }

      span {
        &+span {
          margin-left: 9px;
        }

        img {
          width: 25px !important;
          height: auto !important;
        }
      }
    }


    >div {
      position: relative;
      color-scheme: light dark;
      color: rgba(255, 255, 255, 0.87);
      text-align: initial;
      border-radius: 12px;
      box-shadow: 0 3px 9px rgba(0, 0, 0, .2);



      a {
        position: absolute;
        top: 10px;
        left: 15px;
        font-size: 1.5rem;
        filter: drop-shadow(0 3px 3px #000);
        color: white;
      }

      p {
        padding: 0.6em 1em;
        font-size: 1.1rem;


        &:first-child {
          position: relative;
          margin-bottom: -47.5553px;
          z-index: 11111;
          filter: drop-shadow(0 3px 3px #000);
        }

        &:last-child {
          font-size: 0.9rem;
        }
      }

      img {
        width: 100%;
        height: auto;
        z-index: -1;
        border-top-left-radius: 12px;
        border-top-right-radius: 12px;
      }
    }
  }

  .loot {
    $height: 60px;
    $width: 60px;
    $borderRadius: 5px;

    p {
      padding: 0 !important;
    }



    &--two-star {
      height: $height;
      width: $width;
      border-radius: $borderRadius;
      background-image: url('@/assets/img/backgrounds/two-star.png');
      background-size: cover;
    }

    &--three-star {
      height: $height;
      width: $width;
      border-radius: $borderRadius;
      background-image: url('@/assets/img/backgrounds/three-star.png');
      background-size: cover;
    }

    &--four-star {
      height: $height;
      width: $width;
      border-radius: $borderRadius;
      background-image: url('@/assets/img/backgrounds/four-star.png');
      background-size: cover;
    }

    &--five-star {
      height: $height;
      width: $width;
      border-radius: $borderRadius;
      background-image: url('@/assets/img/backgrounds/five-star.png');
      background-size: cover;
    }
  }
</style>
