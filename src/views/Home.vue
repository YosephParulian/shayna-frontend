<template>
  <div class="home">
    <HeaderToko />
    <HeroToko />
    <WomenBanner />
    <InstagramToko />
    <PartnerLogo />
    <FooterToko />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderToko from '@/components/HeaderToko.vue';
import HeroToko from '@/components/HeroToko.vue';
import WomenBanner from '@/components/WomenBanner.vue';
import InstagramToko from '@/components/InstagramToko.vue';
import PartnerLogo from '@/components/PartnerLogo.vue';
import FooterToko from '@/components/FooterToko.vue';

export default {
  name: 'Home',
  components: {
    HeaderToko,
    HeroToko,
    WomenBanner,
    InstagramToko,
    PartnerLogo,
    FooterToko
  }
}
</script>
