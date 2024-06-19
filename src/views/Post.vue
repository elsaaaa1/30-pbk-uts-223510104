<template>
  <div class="post-container">
    <div class="profile">
      <h1>POSTINGAN</h1>
      <div class="user-selection">
        <label for="users">Pilih User:</label>
        <select id="users" v-model="selectedUserId" @change="loadUserPosts">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
      </div>
      <div class="user-posts">
        <div v-if="loading" class="loading">Loading...</div>
        <div v-else>
          <table>
            <thead>
              <tr>
                <th>Title</th>
                <th>Content</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="post in userPosts" :key="post.id">
                <td>{{ post.title }}</td>
                <td>{{ post.body }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [],
      selectedUserId: null,
      userPosts: [],
      loading: false
    };
  },
  mounted() {
    this.fetchUsers();
  },
  methods: {
    fetchUsers() {
      fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())
        .then(data => {
          this.users = data;
        })
        .catch(error => {
          console.error('Error fetching users:', error);
        });
    },
    loadUserPosts() {
      if (!this.selectedUserId) return;
      this.loading = true;
      fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUserId}`)
        .then(response => response.json())
        .then(data => {
          this.userPosts = data;
          this.loading = false;
        })
        .catch(error => {
          console.error('Error fetching user posts:', error);
          this.loading = false;
        });
    }
  }
};
</script>

<style>

.post-container {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  margin: 0 auto;
  margin-top: 20px;
  margin-bottom: 10px;
}

.profile {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  max-width: 800px;
  width: 100%;
}

.profile h1 {
  color: #d81b60;
  text-align: center;
  font-size: 32px !important;
  font-weight: bold;
  margin-bottom: -5px;
}

.user-selection {
  margin-bottom: 20px;
  text-align: center;
}

.user-selection select {
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #d81b60;
}

.user-posts .loading {
  text-align: center;
  color: #d81b60;
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

td {
  background-color: #fff;
}

td:nth-child(1) {
  color: #d81b60;
}

td:nth-child(2) {
  color: #333;
}
</style>
