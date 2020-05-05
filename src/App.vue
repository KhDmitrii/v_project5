<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <h1>{{ title }}</h1>
        </div>
      </div>
      <div class="row">
        <div class="form-group w-100">
          <label for="name">Имя</label>
          <input type="text" id="name" class="form-control" v-model="name" />
        </div>
        <div class="form-group w-100">
          <label for="surname">Фамилия</label>
          <input
            type="text"
            id="surname"
            class="form-control"
            v-model="surname"
          />
        </div>
        <div class="form-group w-100">
          <label for="phone">Телефон</label>
          <input type="text" id="phone" class="form-control" v-model="phone" />
        </div>
        <div class="btn btn-success" @click="onSave()">Сохранить</div>
      </div>
      <div class="row mt-1" v-for="note in notes" :key="note.id">
        <div class="col-4">{{ note.name }}</div>
        <div class="col-4">{{ note.surname }}</div>
        <div class="col-4">{{ note.phone }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      title: 'myApp',
      name: '',
      surname: '',
      phone: '',
      notes: [],
    };
  },
  components: {},
  methods: {
    async onSave() {
      let note = {
        name: this.name,
        surname: this.surname,
        phone: this.phone,
        notes: [],
      };
      try {
        await this.$http.post('http://localhost:3000/notes', note);
        this.updateData();
      } catch (err) {
        console.error(err);
      }
    },
    async updateData() {
      try {
        await this.$http
          .get('http://localhost:3000/notes')
          .then((res) => res.json())
          .then((res) => (this.notes = res));
      } catch (err) {
        console.error(err);
      }
    },
  },
  created() {
    this.updateData();
  },
};
</script>
