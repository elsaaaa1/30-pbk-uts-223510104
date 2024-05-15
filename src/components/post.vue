<template>
  <div class="app">
    <nav class="navbar">
      <h1>POST</h1>
    </nav>
    <div class="profile">
      <div>
        <label for="users">Pilih User:</label>
        <select id="users" v-model="selectedUserId" @change="loadUserPosts">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
      </div>
      <div class="user-posts">
        <div v-if="loading">Loading...</div>
        <div v-else>
          <div v-for="post in userPosts" :key="post.id" class="post">
            <h2>{{ post.title }}</h2>
            <p>{{ post.body }}</p>
            <a :href="`https://jsonplaceholder.typicode.com/posts/${post.id}`" target="_blank">View Post</a>
            <button @click="editPost(post)">Edit</button>
          </div>
        </div>
      </div>
      <div v-if="editingPost" class="edit-post">
        <h2>Edit Post</h2>
        <input v-model="editingPost.title" placeholder="Post Title" />
        <textarea v-model="editingPost.body" placeholder="Post Body"></textarea>
        <button @click="savePost">Save</button>
        <button @click="cancelEdit">Cancel</button>
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
      loading: false,
      editingPost: null,
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
    },
    editPost(post) {
      this.editingPost = { ...post };
    },
    savePost() {
      // Logic to save the edited post, e.g., via an API request
      this.userPosts = this.userPosts.map(post =>
        post.id === this.editingPost.id ? this.editingPost : post
      );
      this.editingPost = null;
    },
    cancelEdit() {
      this.editingPost = null;
    },
  },
};
</script>

<style>
.app {
  font-family: 'Segoe UI', 'Tahoma', 'Geneva', 'Verdana', sans-serif;
}

.navbar {
  background-color: #1100;
  padding: 20px;
  text-align: center;
  color: #100e0f;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.profile {
  padding: 20px;
  background-color: rgba(17, 0, 0, 0.1);
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.user-posts {
  margin-top: 20px;
}

.user-posts .post {
  margin-bottom: 20px;
  background-color: #222;
  padding: 10px;
  border-radius: 5px;
}

.user-posts h2 {
  color: #ffffff;
}

.user-posts p {
  color: #ffffff;
}

.user-posts a {
  display: block;
  margin-top: 10px;
  color: #1e90ff;
  text-decoration: none;
}

.user-posts a:hover {
  text-decoration: underline;
}

.user-posts button {
  margin-top: 10px;
  padding: 5px 10px;
  background-color: #1e90ff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.user-posts button:hover {
  background-color: #63b3ed;
}

.edit-post {
  margin-top: 20px;
  background-color: #444;
  padding: 20px;
  border-radius: 10px;
}

.edit-post h2 {
  color: #fff;git
}

.edit-post input,
.edit-post textarea {
  width: 100%;
  padding: 10px;
  margin-top: 10px;
  border: none;
  border-radius: 5px;
}

.edit-post button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #1e90ff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.edit-post button:hover {
  background-color: #63b3ed;
}
</style>
