<template>
  <div>
    <v-form ref="form" @submit.prevent="send">
      <v-text-field
      color="#e45b0c"
        v-model="text"
        label="Message..."
        required
        outlined
        :rules="rules"
        append-icon="mdi-send-circle-outline"
        @focus="typing"
        @blur="resetValidation"
        @click:append="send"
        maxlength="400"
        counter
      />
    </v-form>
  </div>
</template>

<script>
import { mapActions, mapGetters } from "vuex";

export default {
  data(){
    return {
      text: "",
      rules: [
        v => !!v || "Enter your message"],
    }
  },
  computed: {
    ...mapGetters(["typingStatus"]),
  },
  methods: {
    ...mapActions(["createMessage", "setTypingStatus"]),
    send() {
      console.log(navigator.onLine)
      if (this.$refs.form.validate()) {
        this.createMessage(this.text);
        this.text = "";
        this.setTypingStatus(false);
        this.resetValidation();
      }
    },
    resetValidation() {
      this.$refs.form.resetValidation();
      this.setTypingStatus(false);
    },
    typing() {
      if (!this.typingStatus) {
        this.setTypingStatus(true);
      }
    },
  },
};
</script>
