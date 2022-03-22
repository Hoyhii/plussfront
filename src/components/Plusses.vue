<template>
  <div class="container">
    <div class="border border-dark">
      <h1>Plusses</h1>
    </div>

    <table class="table table-hover table-dark">
      <thead>
        <tr>
          <th>Pluss</th>
          <th>Műveletek</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="pluss in plusses" v-bind:key="pluss.id">
          <td>{{ pluss.name }}</td>
          <td>
            <button @click="loadEditData(pluss.id)" class="btn btn-primary">
              Szerkesztés
            </button>
            <button @click="deletePluss(pluss.id)" class="btn btn-danger">
              Törlés
            </button>
          </td>
        </tr>
        <tr>
          <td>
            <input type="text" v-model="pluss.name" class="form-control" />
          </td>
          <td>
            <button v-if="ujAdat" @click="newPluss" class="btn btn-primary">
              Létrehozás
            </button>
            <button
              v-if="!ujAdat"
              @click="saveEditData"
              class="btn btn-primary"
            >
              Mentés
            </button>
            <button v-if="!ujAdat" @click="clearForm" class="btn btn-light">
              Mégse
            </button>
            <button v-if="ujAdat" @click="clearForm" class="btn btn-light">
              Tisztítás
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "Plusses",
  data() {
    return {
      ujAdat: true,
      pluss: {
        id: null,
        name: "",
      },
      plusses: [],
    };
  },
  methods: {
    async loadData() {
      let Response = await fetch("http://127.0.0.1:8000/api/plusses");
      let data = await Response.json();
      this.plusses = data;
    },
    async newPluss() {
      await fetch("http://127.0.0.1:8000/api/plusses", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
        },
        body: JSON.stringify(this.pluss),
      });
      await this.loadData();
      this.clearForm();
    },
    async deletePluss(id) {
      let Response = await fetch(`http://127.0.0.1:8000/api/plusses/${id}`, {
        method: "DELETE",
      });
      console.log(Response);
      await this.loadData();
    },
    async loadEditData(id) {
      let Response = await fetch(`http://127.0.0.1:8000/api/plusses/${id}`);
      let data = await Response.json();
      this.pluss = { ...data };
      this.ujAdat = false;
    },
    async saveEditData() {
      await fetch(`http://127.0.0.1:8000/api/plusses/${this.pluss.id}`, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
        },
        body: JSON.stringify(this.pluss),
      });
      this.loadData();
      this.clearForm();
      this.ujAdat = true;
    },
    clearForm() {
      this.pluss = {
        id: null,
        name: "",
      };
      this.ujAdat = true;
    },
  },
  mounted() {
    this.loadData();
  },
};
</script>