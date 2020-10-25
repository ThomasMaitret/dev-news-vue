<template>
  <ul class="posts">
    <li v-for="post in sortedPosts" :key="post.id" class="post">
      <img
        :src="getImageSrc(post.domain)"
        :alt="post.domain"
        class="site-icon"
        loading="lazy"
      />
      <a :href="post.url" target="_blank" rel="noopener" class="link">
        {{ post.title }}
      </a>
      <span> ({{ post.domain }})</span>
      <br />
      <a
        :href="getSubredditLink(post.permalink)"
        target="_blank"
        rel="noopener"
        class="subreddit"
      >
        {{ post.subreddit }}
      </a>
      • <span>{{ getPostDate(post.created) }}</span>
    </li>
  </ul>
</template>

<script>
import { ref } from "vue";

const SUBREDDITS = [
  "angular2",
  "node",
  "javascript",
  "webdev",
  "frontend",
  "coding",
  "css",
  "html5",
];
const LIMIT = 5;

export default {
  name: "Posts",
  setup() {
    const cachedPosts = localStorage.getItem("posts");
    const posts = ref(cachedPosts ? JSON.parse(cachedPosts) : []);

    async function getPosts() {
      try {
        const listings = await Promise.all(
          SUBREDDITS.map(async (sub) => {
            const response = await fetch(
              `https://www.reddit.com/r/${sub}/hot.json?limit=${LIMIT}`
            );
            return await response.json();
          })
        );
        posts.value = getPostsFromListings(listings);

        if (posts.value) {
          localStorage.setItem("posts", JSON.stringify(posts.value));
        }
      } catch (error) {
        throw new Error(error);
      }
    }
    getPosts();

    return {
      posts,
    };
  },
  methods: {
    getImageSrc(domainURL) {
      const domain = domainURL.startsWith("self.") ? "reddit.com" : domainURL;
      return `https://www.google.com/s2/favicons?domain_url=${domain}&sz=16`;
    },
    getSubredditLink(permalink) {
      return `https://reddit.com${permalink}`;
    },
    getPostDate(creationDate) {
      return new Date(creationDate * 1000).toLocaleDateString();
    },
  },
  computed: {
    sortedPosts: function () {
      return this.posts.sort(
        (a, b) => new Date(b.created * 1000) - new Date(a.created * 1000)
      );
    },
  },
};

function getPostsFromListings(listings) {
  return listings.reduce((array, listing) => {
    array.push(
      ...Object.values(listing.data.children).map((item) => item.data)
    );
    return array;
  }, []);
}
</script>

<style>
.posts {
  margin: 1.5rem 0;
  padding: 0;
}

.post {
  list-style: none;
  margin: 2rem 0;
  content-visibility: auto;
  contain-intrinsic-size: 75px;
}

.post:first-child {
  margin-top: 0;
}

.link {
  font-size: 1.2rem;
  font-weight: 600;
  text-decoration: none;
  color: var(--primary-color);
  line-height: 2rem;
}

span {
  color: var(--secondary-color);
}

.subreddit {
  display: inline-block;
  margin-top: 0.5rem;
  font-size: 0.85rem;
}

.site-icon {
  margin-right: 0.5rem;
  width: 16px;
  height: 16px;
}

@media (min-width: 1024px) {
  .link {
    font-size: 1.5rem;
  }
}
</style>
