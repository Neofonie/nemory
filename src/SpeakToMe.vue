<template>
  <div>
    <h5>Voice detection: {{ voice.state }}</h5>
    <p>{{ voice.confidence }} / {{ voice.detected }}</p>
  </div>
</template>

<script>
import SpeakToMe from "speaktome-api/build/stm_web.min.js";

export default {
  name: "speak-to-me",

  props: ["onDetect"],

  data() {
    return {
      voice: {
        state: "",
        confidence: "",
        detected: ""
      }
    };
  },

  mounted() {
    const stm = SpeakToMe({
      listener: msg => {
        console.log("listener", msg);
        this.voice.state = msg.state;

        if (msg.data && msg.data.length > 0) {
          const result = msg.data[0].text;
          // is it a number ?
          if (result === parseInt(result).toString()) {
            this.voice.confidence = msg.data[0].confidence;
            this.voice.detected = result;
            this.onDetect(result);
          }
        }
        if (msg.state === "ready") {
          stm.listen();
        }
      }
    });

    stm.listen();
    this.voice.state = "init";
  }
};
</script>

<style scoped></style>
