<template>
  <v-row justify="center" no-gutters>
    <p v-if="isSystemMessage" class="text-center font-italic system">
      {{ message.text }}
    </p>
    <v-col v-else class="msg-wrapper">
      <v-row
        no-gutters
        justify="space-between"
        class="msg"
        :class="{ owner }"
        @contextmenu.prevent="$emit('log', message.time)"
      >
        <v-avatar class="mr-3">
          
          <img :src="require(`../static/comment_2.png`)" alt="John" />
        </v-avatar>
        <v-col>
          <span class="font-weight-bold">{{ message.name }}</span>
          <p class="mb-0">
            {{ message.text }}
          </p>
        </v-col>
        <v-col cols="auto">
          <span class="msg__date ml-3">{{ message.time }}</span>
          
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: {
    message: {
      type: Object,
      default: () => {},
    },
    // user: {
    //   type: Object,
    //   default: () => {},
    // },
    owner: {
      type: Boolean,
    },
  },
  computed: {
    isSystemMessage() {
      return this.message.name === "admin";
    },
  },
};
</script>

<style lang="scss" scoped>
.system {
  margin-bottom: 1rem;
  color: #fff;

  p {
    margin-bottom: 1rem;
  }
}

.msg-wrapper {
  display: flex;
  flex-direction: column;
  overflow-x: hidden;
}

.msg {
  padding: 1rem;
  width: 60%;
  margin: 0 1rem;
  box-shadow: 0 1px 0 0 rgba(50, 50, 50, 0.3);
  border-radius: 20px;
  background: linear-gradient(170deg, #ec6414, #e40c8a);
  color: #fff;
  position: relative;
  word-break: break-all;
  margin-bottom: 1rem;
  animation: flyin2 0.25s linear forwards;

  &__date {
    text-decoration: underline;
  }
}

.owner {
  background: linear-gradient(170deg, #e004a9, #810837);
  color: #fff;
  align-self: flex-end;
  border-radius: 20px;
  animation: flyin 0.25s linear forwards;
}
@keyframes flyin {
  0% {
    transform: translateX(50%);
  }
  100% {
    transform: translateX(0);
  }
}
@keyframes flyin2 {
  0% {
    transform: translateX(-50%);
  }
  100% {
    transform: translateX(0);
  }
}

@media (max-width: 400px) {
  .msg {
    width: 90%;
  }
}
</style>
