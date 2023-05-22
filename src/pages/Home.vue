<template>
  <div class='container mx-auto'>
    <div>
      <h2>Available Domains</h2>

      <div class='flex flex-row flex-wrap'>
        <DomainCard
          v-for='item in availableDomains'
          :key='item.location.name'
          :item='item'
          :unavailable='false'
        />
      </div>
    </div>


    <div>
      <h2>Unavailable Domains</h2>

      <div class='flex flex-row flex-wrap'>
        <DomainCard
          v-for='item in unavailableDomains'
          :key='item.location.name'
          :item='item'
          :unavailable='true'
        />
      </div>
    </div>
  </div>
</template>




<script setup lang='ts'>
  import { ref, onMounted } from 'vue';
  import moment from 'moment'
  import DomainCard from '@/components/molecules/DomainCard/DomainCard.vue'
  import db from './../database/domains';

  let dayMap: any = {
    1: 'Monday',
    2: 'Tuesday',
    3: 'Wednesday',
    4: 'Thursday',
    5: 'Friday',
    6: 'Saturday',
    0: 'Sunday'
  }

  const availableDomains: any = ref([])
  const unavailableDomains: any = ref([])


  const currentDayMoment = moment()
  const serverReset = moment().hour(4).minute(0).second(0)


  onMounted(() => {
    availableDomains.value = getAvailableDomains(db)
    unavailableDomains.value = getUnavailableDomains(db)
  })

  const getAvailableDomains = (db: any) => {
    const locations = Object.entries(db.domains).flatMap(([domain, singleDomain]) => {
      return singleDomain.flatMap((_domain: any) => {
        return _domain.locations.filter((location: any) => {
          return location.availability.includes(dayMap[calculateTime()])
        }).map((location: any) => {
          const { name, previewImageUrl, recommendedElements, wikiUrl } = _domain
          return {
            domainName: name,
            domainPreviewImage: previewImageUrl,
            recommendedElements,
            wikiUrl,
            location,
          }
        })
      })
    })

    return locations;
  }



  const getUnavailableDomains = (db: any) => {
    const locations = Object.entries(db.domains).flatMap(([domain, singleDomain]) => {
      return singleDomain.flatMap((_domain: any) => {
        return _domain.locations.filter((location: any) => {
          return !location.availability.includes(dayMap[calculateTime()])
        }).map((location: any) => {
          const { name, previewImageUrl, recommendedElements, wikiUrl } = _domain
          return {
            domainName: name,
            domainPreviewImage: previewImageUrl,
            recommendedElements,
            wikiUrl,
            location,
          }
        })
      })
    })

    return locations;
  }



  const calculateTime = () => {
    if (currentDayMoment.isBefore(serverReset)) {
      return moment().subtract(5, 'hour').day()
    } else {
      return moment().day()
    }
  }
</script>





<style scoped lang='scss'>
  h1 {
    font-size: 3.2em;
    line-height: 1.1;
  }

  h2 {
    font-size: 2.7em;
    line-height: 1.1;
  }

  h3 {
    font-size: 2.3em;
    line-height: 1.1;
  }

  h4 {
    font-size: 1.7em;
    line-height: 1.1;
  }
</style>
