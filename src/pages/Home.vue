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

  interface Domain {
    name: string;
    previewImageUrl: string;
    recommendedElements: any[];
    wikiUrl: string;
    locations: Location[];
  }

  interface Location {
    availability: string[];
  }

  interface CustomLocation {
    domainName: string;
    domainPreviewImage: string;
    recommendedElements: any[];
    wikiUrl: string;
    location: Location;
  }


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

  // Domain contains a dictionary of keys of type Domain[]
  const getAvailableDomains = (db: { domains: { [key: string]: Domain[] } }): CustomLocation[] => {
    const currentDay = calculateTime();

    return Object.entries(db.domains)
      .flatMap(([_, singleDomain]) =>
        singleDomain.flatMap((domain) =>
          domain.locations
            .filter((location) => location.availability.includes(dayMap[currentDay]))
            .map((location) => ({
              domainName: domain.name,
              domainPreviewImage: domain.previewImageUrl,
              recommendedElements: domain.recommendedElements,
              wikiUrl: domain.wikiUrl,
              location,
            }))
        )
      );
  };



  const getUnavailableDomains = (db: { domains: { [key: string]: Domain[] } }): CustomLocation[] => {
    const currentDay = calculateTime();

    return Object.entries(db.domains)
      .flatMap(([_, singleDomain]) =>
        singleDomain.flatMap((domain) =>
          domain.locations
            .filter((location) => !location.availability.includes(dayMap[currentDay]))
            .map((location) => ({
              domainName: domain.name,
              domainPreviewImage: domain.previewImageUrl,
              recommendedElements: domain.recommendedElements,
              wikiUrl: domain.wikiUrl,
              location,
            }))
        )
      );
  };



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
