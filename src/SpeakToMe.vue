<template>
  <div>
    <h5>Voice detection:</h5>
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
        confidence: "",
        detected: ""
      }
    };
  },

  mounted() {
    const stm = SpeakToMe({
      listener: msg => {
        console.log("listener", msg);
        if (msg.data && msg.data.length > 0) {
          console.log(msg.data[0].confidence + ": " + msg.data[0].text);
          if (msg.data[0].text === parseInt(msg.data[0].text).toString()) {
            this.voice.confidence = msg.data[0].confidence;
            this.voice.detected = msg.data[0].text;
            this.onDetect(msg.data[0].text);
          }
        }
        if (msg.state === "ready") {
          stm.listen();
        }
      }
    });

    stm.listen();
  }
};
</script>

<style scoped></style>
