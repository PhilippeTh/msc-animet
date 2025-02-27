<template>
  <v-card v-if="getMapTimeSettings.Step !== null">
    <v-card-title class="text-subtitle-2">
      {{ $t("MP4ExportTitle") }}
    </v-card-title>
    <v-card-subtitle>
      {{ $t("MP4ExportSubtitle") }}
    </v-card-subtitle>

    <v-row class="mx-4 mb-2" justify="center">
      <video
        :src="getMP4URL"
        :style="getExportStyle"
        controls
        autoplay
        loop
      ></video>
    </v-row>

    <v-card-actions class="mt-4">
      <v-btn block color="primary" @click="downloadMP4()" class="text-none">
        {{ $t("MP4ExportDownload") }} [{{
          this.formatBytes(this.getOutputSize, 0)
        }}]
        <v-icon dark class="ml-4"> mdi-download </v-icon>
      </v-btn>
    </v-card-actions>
    <a id="MP4-download" :download="exportName"></a>
  </v-card>
</template>

<script>
import { mapGetters } from "vuex";

export default {
  computed: {
    ...mapGetters("Layers", [
      "getAnimationTitle",
      "getExportStyle",
      "getMapTimeSettings",
      "getMP4URL",
      "getOutputDate",
      "getOutputSize",
    ]),
    exportName() {
      const dateIn = this.getOutputDate;
      const yyyy = dateIn.getFullYear();
      const mm = dateIn.getMonth() + 1; // getMonth() is zero-based
      const dd = dateIn.getDate();
      const hh = dateIn.getHours();
      const mi = dateIn.getMinutes();
      const finalMonth = (mm < 10 ? "0" : "") + mm;
      const finalDays = (dd < 10 ? "0" : "") + dd;
      const finalHours = (hh < 10 ? "0" : "") + hh;
      const finalMinutes = (mi < 10 ? "0" : "") + mi;
      const finalDate = String(
        yyyy + finalMonth + finalDays + "T" + finalHours + finalMinutes
      );
      let animationTitle = this.getAnimationTitle;
      if (animationTitle !== "") {
        animationTitle = animationTitle.replace("^", "");
        animationTitle = animationTitle.replace(",", ".");
        animationTitle = animationTitle
          .normalize("NFD")
          .replace(/[\u0300-\u036f]/g, "");
        animationTitle = animationTitle.replace(/[^a-zA-Z0-9.]/g, " ");
        animationTitle = animationTitle.replace(/\s+/g, "_");
        animationTitle = animationTitle.replace(/[^a-zA-Z0-9]$/, "");
        animationTitle = "_" + animationTitle;
      }
      return "MSC-AniMet_" + finalDate + "Z" + animationTitle + ".mp4";
    },
  },
  methods: {
    downloadMP4: function () {
      var MP4link = document.getElementById("MP4-download");
      MP4link.href = this.getMP4URL;
      MP4link.click();
    },
    formatBytes(bytes, decimals = 2) {
      if (bytes === 0) return "0 Bytes";

      const k = 1024;
      var dm = decimals < 0 ? 0 : decimals;
      const sizes = ["Bytes", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB"];

      const i = Math.floor(Math.log(bytes) / Math.log(k));
      if (i > 1) {
        dm = 1;
      }
      return parseFloat((bytes / Math.pow(k, i)).toFixed(dm)) + " " + sizes[i];
    },
  },
};
</script>
