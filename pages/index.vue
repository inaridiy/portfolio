<template>
  <v-main>
    <v-container>
      <div>
        <h1 class="text-h2">かなり適当なポートフォリオ</h1>
        <p>6時間でどれだけのサイトが作れるのか気になったのでやってみた</p>
      </div>
      <v-row>
        <v-col v-for="work in works" :key="work.slug" cols="12" sm="6" lg="4">
          <v-hover v-slot="{ hover }">
            <v-card
              dark
              :elevation="hover ? 12 : 2"
              @click="closeoropen(work.slug)"
            >
              <v-img
                :src="work.image"
                class="align-end card-img"
                :class="{ 'on-hover': hover }"
                :aspect-ratio="openWork === work.slug || 16 / 9"
                ><div class="fill-height bottom-gradient"></div
              ></v-img>
              <v-card-title class="text-h4 text-xl-h3 font-weight-bold">{{
                work.title
              }}</v-card-title>
              <v-chip class="ma-1" v-for="tag in work.tags" :key="tag">
                {{ tag }}
              </v-chip>
              <v-card-text
                v-if="openWork === work.slug"
                class="font-weight-bold"
              >
                <nuxt-content :document="work"
              /></v-card-text>
            </v-card>
          </v-hover>
        </v-col>
      </v-row>
    </v-container>
  </v-main>
</template>

<script>
export default {
  data: () => ({ openWork: "" }),
  async asyncData({ $content }) {
    const works = await $content("works").fetch();
    return { works };
  },
  computed: {
    isViewing() {
      return this.$route.params;
    }
  },
  methods: {
    closeoropen(work_slug) {
      if (this.openWork === work_slug) {
        this.openWork = "";
      } else {
        this.openWork = work_slug;
      }
    }
  }
};
</script>

<style scoped>
.card-img {
  transition: opacity 0.1s ease-in-out;
}

.card-img:not(.on-hover) {
  opacity: 0.7;
}
.top-enter-active,
.top-leave-active {
  transform: translate(0px, 0px);
  transition: transform 225ms cubic-bezier(0, 0, 0.2, 1) 0ms;
}

.top-enter,
.top-leave-to {
  transform: translateY(-100vh) translateY(0px);
}
</style>
