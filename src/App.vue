<script>
import Map from "./components/Map.vue";

export default {

  data() {
    return {
      user: '',
      map: '',
      tabs: ["Information", "Contact", "Address"],
      currentTab: 0,
    }
  },
  methods: {
    async getUser() {
      const res = await fetch("https://randomuser.me/api/")
      const { results } = await res.json();
      this.user = results[0]
      console.log(this.user.location.coordinates.longitude, this.user.location.coordinates.latitude);
    },
  },

  components: {
    Map,
  },

  created: function () {
    this.getUser()
  },
}
</script>

<template >
  <div class="container" v-if="user">
    <button class="random-btn" @click="getUser">Generate Random User</button>
    <div class="user">
      <img class="user-img" :src="user.picture.medium" :alt="user.name.first" />
      <h1 class="user-name">{{ user.name.first }} {{ user.name.last }}</h1>
      <div class="user-info">
        <div class="btn-group">
          <button v-for="(tab, index) in tabs" @click="currentTab = index" class="btn-tab"
            :class="{ active: currentTab === index }">{{
                tab
            }}</button>
        </div>
        <div class="tab-content">
          <div v-show="currentTab === 0">
            <table>
              <tbody>
                <tr>
                  <th>Full Name</th>
                  <td>{{ user.name.title }} {{ user.name.first }} {{ user.name.last }}</td>
                </tr>
                <tr>
                  <th>Gender</th>
                  <td>{{ user.gender }}</td>
                </tr>
                <tr>
                  <th>Age</th>
                  <td>{{ user.dob.age }}</td>
                </tr>
                <tr>
                  <th>DOB</th>
                  <td>{{ new Date(user.dob.date).toLocaleString("en") }}</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div v-show="currentTab === 1">
            <table>
              <tbody>
                <tr>
                  <th>Email</th>
                  <td>{{ user.email }}</td>
                </tr>
                <tr>
                  <th>Phone</th>
                  <td>{{ user.phone }}</td>
                </tr>
                <tr>
                  <th>Cell</th>
                  <td>{{ user.cell }}</td>
                </tr>
                <tr>
                  <th>Address</th>
                  <td>{{ user.location.country }}, {{ user.location.state }}, {{ user.location.city }}, {{
                      user.location.street.name
                  }}, {{ user.location.street.number }}</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div v-show="currentTab === 2">
            <Map :lat="user.location.coordinates.latitude" :lng="user.location.coordinates.longitude" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style >
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Poppins", sans-serif;
  background-color: #f9f9f9;
}

button {
  font-family: "Poppins", sans-serif;
}

.container {
  width: min(600px, 95%);
  margin: 3rem auto;
}

.random-btn {
  display: block;
  width: 100%;
  color: white;
  padding: 1.5rem;
  font-size: 1rem;
  background-color: #13334c;
  border-radius: 10px 10px 0 0;
  cursor: pointer;
  border: none;
}

.user {
  border: 1px solid #e5e5e5;
  text-align: center;
  padding: 2rem;
  background-color: white;
  border-radius: 0 0 10px 10px;
}

.user-img {
  aspect-ratio: 1/1;
  width: 75px;
  border-radius: 50%;
  margin-bottom: 1.5rem;
}

.user-name {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 1.5rem;
  color: #005792;
}

.btn-group {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  margin-bottom: 2rem;
}

.btn-tab {
  background-color: transparent;
  flex-basis: 33%;
  border: none;
  outline: none;
  padding: 0.5rem 1rem;
  cursor: pointer;
  font-size: 1rem;
  border-bottom: 3px solid transparent;
  color: #13334c;
}

.btn-tab.active {
  border-bottom: 3px solid #fd5f00;
}

table {
  width: 100%;
  border-collapse: collapse;
}

tr:nth-child(odd) {
  background-color: #f5f5f5;
}

th,
td {
  padding: 0.5rem;
  text-align: left;
  font-size: 0.8125rem;
  text-transform: capitalize;
}

th {
  width: 30%;
}

.map {
  height: 300px;
  width: 100%;
}

@media (max-width: 576px) {
  .user {
    padding: 1rem;
  }

  .btn-group {
    gap: 0rem;
  }

  .btn-tab {
    padding: 0.5rem;
    font-size: 0.875rem;
  }
}
</style>