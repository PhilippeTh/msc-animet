<template>
  <v-app-bar app>
    <v-toolbar-title>
      {{ $t("MSCAnimet") }}
    </v-toolbar-title>
    <v-spacer></v-spacer>

    <v-btn text @click="$vuetify.goTo('#mapComponent')">
      <v-icon>mdi-map</v-icon>
      <span
        class="d-none d-sm-none d-md-flex text-transform-none"
        v-text="$t('AppBarMap')"
      ></span>
    </v-btn>
    <v-divider></v-divider>
    <v-btn text @click="$vuetify.goTo('#geoMetTree')">
      <v-icon>mdi-file-tree</v-icon>
      <span
        class="d-none d-sm-none d-md-flex text-transform-none"
        v-text="$t('AppBarTree')"
      ></span>
    </v-btn>
    <v-divider></v-divider>
    <v-btn
      :disabled="getMapTimeSettings.Step === null"
      text
      @click="$vuetify.goTo('#createMP4Controls')"
    >
      <v-icon>mdi-cog</v-icon>
      <span
        class="d-none d-sm-none d-md-flex text-transform-none"
        v-text="$t('AppBarCreate')"
      ></span>
    </v-btn>
    <v-divider></v-divider>
    <v-btn
      :disabled="getMapTimeSettings.Step === null || getMP4URL == 'null'"
      text
      @click="$vuetify.goTo('#MP4exportid')"
    >
      <v-icon>mdi-movie-open-play</v-icon>
      <span
        class="d-none d-sm-none d-md-flex text-transform-none"
        v-text="$t('AppBarExport')"
      ></span>
    </v-btn>
    <v-spacer></v-spacer>

    <PermaLink id="permaLink" class="my-4" />

    <v-tooltip bottom>
      <template v-slot:activator="{ on, attrs }">
        <v-btn v-bind="attrs" v-on="on" icon @click="toggleThemeDarkMode">
          <v-icon
            :style="{
              transform: `rotate(${$vuetify.theme.dark ? 0 : -190}deg)`,
            }"
          >
            {{ "mdi-theme-light-dark" }}
          </v-icon>
        </v-btn>
      </template>
      <span
        >{{
          $vuetify.theme.dark
            ? $t("MP4CreateLightMode")
            : $t("MP4CreateDarkMode")
        }}
        Mode</span
      >
    </v-tooltip>

    <v-btn icon @click="changeLang">
      {{ this.getFlagLang }}
    </v-btn>

    <v-tooltip bottom>
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          v-bind="attrs"
          v-on="on"
          :href="$t('documentationURL')"
          target="_blank"
          icon
        >
          <v-icon> mdi-information-outline </v-icon>
        </v-btn>
      </template>
      <span>{{ $t("userDoc") }}</span>
    </v-tooltip>
  </v-app-bar>
</template>

<script>
import { mapGetters } from "vuex";
import PermaLink from "./PermaLink.vue";

export default {
  name: "AppBar",
  components: {
    PermaLink,
  },
  methods: {
    toggleThemeDarkMode() {
      this.$vuetify.theme.dark = !this.$vuetify.theme.dark;
      // this.$root.$emit( 'darkModeOSM' , !this.$vuetify.theme.dark )
    },
    changeLang() {
      if (this.$i18n.locale === "en") {
        this.$store.dispatch("Layers/setLang", "fr");
        this.$i18n.locale = "fr";
        this.$vuetify.current = "fr";
      } else {
        this.$store.dispatch("Layers/setLang", "en");
        this.$i18n.locale = "en";
        this.$vuetify.current = "en";
      }
      this.$root.$emit("localeChange");
    },
  },
  computed: {
    ...mapGetters("Layers", ["getMapTimeSettings", "getMP4URL"]),
    getFlagLang() {
      if (this.$i18n.locale === "fr") {
        return "EN";
      } else {
        return "FR";
      }
    },
  },
};
</script>

<style scoped>
.text-transform-none {
  text-transform: none;
}
</style>
