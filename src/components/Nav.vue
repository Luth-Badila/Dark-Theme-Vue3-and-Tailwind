<template>
  <div class="container mx-auto px-4">
    <div class="tab-menu">
      <div v-for="(tab, index) in tabs" :key="index" :class="['tab', { active: activeTab === index }]" @click="setActiveTab(index)">
        {{ tab.title }}
      </div>
    </div>

    <div class="tab-content">
      <div v-for="(tab, index) in tabs" :key="index" v-show="activeTab === index">
        <div class="post" v-for="post in tab.posts" :key="post.id">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tabs: [
        { title: "Tab 1", posts: [] },
        { title: "Tab 2", posts: [] },
        { title: "Tab 3", posts: [] },
      ],
      activeTab: 0,
    };
  },
  mounted() {
    this.fetchPosts();
  },
  methods: {
    fetchPosts() {
      fetch("https://jsonplaceholder.typicode.com/posts")
        .then((response) => response.json())
        .then((data) => {
          this.tabs.forEach((tab, index) => {
            // Assuming each tab should display a different set of posts
            tab.posts = data.slice(index * 5, (index + 1) * 5);
          });
        })
        .catch((error) => {
          console.error("Error fetching posts:", error);
        });
    },
    setActiveTab(index) {
      this.activeTab = index;
    },
  },
};
</script>
