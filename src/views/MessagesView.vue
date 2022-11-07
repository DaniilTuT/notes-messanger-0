<template>
  <main>
    <div class="container">
      <div v-if="messages && messages.length > 0" class="messages">
        <div v-for="(message, idy) in messages"
        class="blockN">
          <div
               :key="idy"
               class="message">
            <div class="text">
              <p>{{ message.message }}</p>
            </div>
            <div class="time">
              <time>{{ message.time }}</time>
            </div>
          </div>
          <button @click="messages.splice(idy,1); saveMessage()"
          class="btnD">
            Delete
          </button>
        </div>


      </div>
      <div class="net" v-else>
        <span>Сообщений нет</span>
      </div>
      <div class="footer">
        <div class="input">
          <input
              @keyup.enter="addMessage"
              v-model="message1"
              type="text"
              placeholder="Введите сообщение"
          >
        </div>
      </div>
    </div>
  </main>
</template>

<script>
// import { dialogs } from ".\views\DialogsView.vue";
export default {
  props: {},

  data() {
    return {
      dialogs1: [],
      message1: '',
      messages: [],
      idy: null,
      id: this.dialogs,
    };
  },

  computed: {},

  watch: {},

  created() {
    this.dialogs1 = dialogs;
  },

  mounted() {
    this.loadMessage();
  },

  methods: {
    addMessage() {
      this.messages.push({
        message: this.message1,
        time: new Date().toLocaleString(),
        id: new Date().getMilliseconds(),
      });
      this.message1 = '';
      this.saveMessage();
    },

    saveMessage() {
      localStorage.setItem('messages', JSON.stringify(this.messages))
    },
    loadMessage() {
      this.messages = JSON.parse(localStorage.getItem('messages') || '[]')
    },

  },
}
</script>

<style lang="scss" scoped>
.blockN{
  display: flex ;
}

.btnD{
  display: block;
  float: right;
  margin:18px 10px 10px 200px;
  width: 25%;
  height: fit-content;
  padding: 10px;
  border: 0;
  border-radius: 5px;
  font-size: 16px;
  color: #fff;
  background: #2c2c2c;
  outline: none;
  cursor: pointer;

}

.net {
  margin-top: 10px;
  text-align: center;
  height: 95%;
}

.messages {
  margin-top: 10px;
  height: 100%;
  overflow-y: auto;
}

.message {
  margin-left: 10px;
  border-radius: 5px;
  border: 1px solid #545454;
  padding: 10px 15px;
  margin-bottom: 10px;
  width: fit-content;
  height: fit-content;
  display: block;
  float: left;

  .text {
    p {
      text-align: end;
    }
  }

  .time {
    margin-top: 5px;
    text-align: right;
    font-size: 12px;
    color: #999;
  }
}
</style>
