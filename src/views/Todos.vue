<template>
    <div class="todos-container">
      <h1>Daftar Barang</h1>
      <input type="text" v-model="newActivity.name" placeholder="Tambahkan barang baru">
      <div class="datetime-container">
        <label for="datetime">Tanggal & Jam:</label>
        <input id="datetime" type="datetime-local" v-model="newActivity.dateTime">
      </div>
      <button @click="addActivity">Tambah</button>
      <table>
        <thead>
          <tr>
            <th>Barang</th>
            <th>Tanggal & Jam</th>
            <th>Status</th>
            <th>Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(activity, index) in activities" :key="index">
            <td>{{ activity.name }}</td>
            <td>{{ formatDate(activity.dateTime) }}</td>
            <td>
              <input type="checkbox" v-model="activity.completed">
              <span :class="{ 'completed': activity.completed }">{{ activity.completed ? 'Selesai' : 'Belum Selesai' }}</span>
            </td>
            <td><button @click="removeActivity(index)">Hapus</button></td>
          </tr>
        </tbody>
      </table>
    </div>
</template>

<script>
export default {
  data() {
    return {
      newActivity: {
        name: '',
        dateTime: '',
        completed: false
      },
      activities: []
    };
  },
  methods: {
    addActivity() {
      if (this.newActivity.name && this.newActivity.dateTime) {
        this.activities.push({...this.newActivity});
        this.newActivity.name = '';
        this.newActivity.dateTime = '';
        this.newActivity.completed = false;
      }
    },
    removeActivity(index) {
      this.activities.splice(index, 1);
    },
    formatDate(dateTime) {
      if (!dateTime) return '';
      const options = { year: 'numeric', month: 'long', day: 'numeric', hour: '2-digit', minute: '2-digit' };
      return new Date(dateTime).toLocaleDateString(undefined, options);
    }
  }
};
</script>

<style>

.todos-container {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 600px;
  width: 100%;
  margin: 0 auto;
  margin-top: 50px;
  border: 0.5px solid #d81b60;
}

.todos-container h1 {
  color: #d81b60;
  font-size: 32px !important;
  text-align: center;
  font-weight: bold !important;
  margin-bottom: -5px;
}

input[type="text"], input[type="datetime-local"] {
  width: calc(100% - 20px);
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #d81b60;
  border-radius: 4px;
}

button {
  background-color: #d81b60;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #c2185b;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th, td {
  border: 1px solid #d81b60;
  padding: 10px;
  text-align: left;
}

th {
  background-color: #f8bbd0;
}

.completed {
  text-decoration: line-through;
}
</style>
