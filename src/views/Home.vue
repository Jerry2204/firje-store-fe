<template>
  <div class="home">
    <Header />
    <Hero />
    <Banner />
    <Insta />
    <PartnerLogo />
    <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "@/components/Header.vue";
import Hero from "@/components/Hero.vue";
import Banner from "@/components/Banner.vue";
import Insta from "@/components/Insta.vue";
import PartnerLogo from "@/components/PartnerLogo.vue";
import Footer from "@/components/Footer.vue";

export default {
  name: "Home",
  components: {
    Header,
    Hero,
    Banner,
    Insta,
    PartnerLogo,
    Footer,
  },
};
</script>
