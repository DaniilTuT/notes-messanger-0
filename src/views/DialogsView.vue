<template>
  <main>
    <div class="container">
      <div class="header">
        <div class="search">
          <input
              type="text"
              placeholder="Поиск"
              v-model="sort"
          >
        </div>
      </div>
      <div v-if=" sortedDialogs &&  sortedDialogs.length" class="dialogs">
      <div class="forD" v-for="(dialog, idx) in sortedDialogs">
        <router-link

            :to="`/messages/${idx}`"
            :key="idx"
        >

          <div class="dialog">
            <div class="title">
              <h3>{{ dialog.title }}</h3>
              <time>{{ dialog.time }}</time>
            </div>
            <div class="last-message">
              <p>{{ dialog.lastM }}</p>
            </div>
          </div>
        </router-link>

        <button class="btnD" @click="dialogs.splice(idx,1); saveDialog()">
          Delete
        </button>
      </div>
      </div>
      <div class="net" v-else>
        <span>Ничего нет</span>
      </div>

      <div class="footer">
        <div class="form">
          <div class="new-dialog">
            <input
                type="text"
                placeholder="Название"
                v-model="title1"
                @keyup.enter="addDialog"
            >
          </div>
          <button @click="this.addDialog">
            Добавить новый дилог
          </button>
        </div>
      </div>
    </div>
  </main>
</template>

<script>

export default {
  props: {},

  data() {
    return {

      dialogs: [],
      title1: '',
      dialog: {},
      sort:'',
      idx: null,
      sortedDialog:[],
    };
  },

  computed: {
    sortedDialogs(){
      return this.dialogs.filter(elem => {return elem.title.toLowerCase().includes(this.sort.toLowerCase());})
    },

  },

  watch: {},

  created() {
  },

  mounted() {
    this.loadDialogs();
  },
  methods: {
    addDialog() {
      this.dialogs.push({
        title: this.title1,
        time: new Date().toLocaleString(),
        lastM: 'Пока нет',
        id: new Date().getMilliseconds(),
        idx: this.idx,
      });
      this.title1 = '';
      this.saveDialog();
    },
    saveDialog() {
      localStorage.setItem('dialogs', JSON.stringify(this.dialogs))
    },
    loadDialogs() {
      this.dialogs = JSON.parse(localStorage.getItem('dialogs' ) || '[]')
    },
  },
}

</script>

<style scoped lang="scss">
.forD{
  display: inline;

}
.btnD{
  margin-left: 80%;
  width: 20%;
  padding: 10px;
  border: 0;
  border-radius: 5px;
  font-size: 16px;
  color: #fff;
  background: #2c2c2c;
  outline: none;
  cursor: pointer;

}
.net{
  margin-top: 10px;
  text-align: center;
  height: 89%;
}

.dialogs {
  margin-top: 10px;
  height: 100%;
  overflow-y: auto;
}

.dialog {
  margin-top: 5px;
  border-radius: 5px;
  padding: 10px 15px;

  .title {
    display: flex;
    justify-content: space-between;
    align-items: end;

    h3 {
      text-overflow: ellipsis;
      overflow: hidden;
      white-space: nowrap;
    }

    time {
      font-size: 12px;
      color: #999;
    }
  }

  .last-message {
    margin-top: 5px;

    p {
      color: #717579;
      text-overflow: ellipsis;
      overflow: hidden;
      white-space: nowrap;
    }
  }

  &:hover {
    background: #2c2c2c;
  }
}

.footer {
  height: 100px;

  .form {
    .new-dialog {
      width: 100%;
    }

    button {
      width: 100%;
      padding: 10px;
      border: 0;
      border-radius: 5px;
      font-size: 16px;
      color: #fff;
      background: #2c2c2c;
      outline: none;
      cursor: pointer;
      margin-top: 10px;
    }
  }
}
</style>
