<template>
  <div>
    <div class="header">
      <status-icon :connected="user.connected" />{{ user.username }}
    </div>

    <ul class="messages">
      <li
        v-for="(message, index) in user.messages"
        :key="index"
        class="message"
      >
        <div v-if="displaySender(message, index)" class="sender">
          {{ message.fromSelf ? "(yourself)" : user.username }}
        </div>
        {{ message.content }}
        <div id="board1" style="width: 400px"></div>
        <div :id="board1" style="width: 400px"></div>
        <span v-html="board1"></span>
      </li>
    </ul>

    <form @submit.prevent="onSubmit" class="form">
      <textarea v-model="input" placeholder="Your message..." class="input" />
      <button :disabled="!isValid" class="send-button">Send</button>
    </form>
  </div>
</template>

<script>
import StatusIcon from "./StatusIcon";
import { h } from 'vue'

export default {
  name: "MessagePanel",
  components: {
    StatusIcon,
  },
  props: {
    user: Object,
  },
  data() {
    return {
      input: "",
      board1: Chessboard('board1', 'start')
    };
  },
  render(){
    return h('div', {id: 'myBoard', style: { width: '400px' }})
  }
  methods: {
    onSubmit() {
      this.$emit("input", this.input);
      this.input = "";
    },
    displaySender(message, index) {
      var board1 = Chessboard('board1', 'start')
      return (
        index === 0 ||
        this.user.messages[index - 1].fromSelf !==
          this.user.messages[index].fromSelf
      );
    },
    
  },
  computed: {
    isValid() {
      return this.input.length > 0;
    },
  },
};
</script>

<style scoped>
.clearfix-7da63{clear:both}.board-b72b1{border:2px solid #404040;box-sizing:content-box}.square-55d63{float:left;position:relative;-webkit-touch-callout:none;-webkit-user-select:none;-khtml-user-select:none;-moz-user-select:none;-ms-user-select:none;user-select:none}.white-1e1d7{background-color:#f0d9b5;color:#b58863}.black-3c85d{background-color:#b58863;color:#f0d9b5}.highlight1-32417,.highlight2-9c5d2{box-shadow:inset 0 0 3px 3px #ff0}.notation-322f9{cursor:default;font-family:"Helvetica Neue",Helvetica,Arial,sans-serif;font-size:14px;position:absolute}.alpha-d2270{bottom:1px;right:3px}.numeric-fc462{top:2px;left:2px}

.header {
  line-height: 40px;
  padding: 10px 20px;
  border-bottom: 1px solid #dddddd;
}

.messages {
  margin: 0;
  padding: 20px;
}

.message {
  list-style: none;
}

.sender {
  font-weight: bold;
  margin-top: 5px;
}

.form {
  padding: 10px;
}

.input {
  width: 80%;
  resize: none;
  padding: 10px;
  line-height: 1.5;
  border-radius: 5px;
  border: 1px solid #000;
}

.send-button {
  vertical-align: top;
}
</style>
