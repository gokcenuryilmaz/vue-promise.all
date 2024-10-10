<template>
  <div id="app">
    <h1>Posts and Comments</h1>
    <div v-if="posts.length">
      <ul>
        <li v-for="post in posts" :key="post.id">
          <h3>Post:</h3>
          <strong>{{ post.title }}</strong>
          <p>{{ post.body }}</p>
          <h4>Comments:</h4>
          <ul>
            <li v-for="comment in filteredComments(post.id)" :key="comment.id">
              <strong>{{ comment.email }}</strong>: {{ comment.body }}
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <p v-else>Loading posts...</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      posts: [], // Post verilerini tutacak dizi
      comments: [], // Yorum verilerini tutacak dizi
    };
  },
  created() {
    this.fetchPostsAndComments(); // Bileşen oluşturulduğunda verileri çek
  },
  methods: {
    // Post ve yorum verilerini çeken metod
    fetchPostsAndComments() {
      const postsPromise = fetch("https://jsonplaceholder.typicode.com/posts")
        .then((response) => {
          if (!response.ok) {
            throw new Error("Posts data could not be fetched");
          }
          return response.json();
        });

      const commentsPromise = fetch("https://jsonplaceholder.typicode.com/comments")
        .then((response) => {
          if (!response.ok) {
            throw new Error("Comments data could not be fetched");
          }
          return response.json();
        });

      // Promise.all ile birden fazla Promise'i bir arada yönet
      Promise.all([postsPromise, commentsPromise])
        .then(([postsData, commentsData]) => {
          this.posts = postsData; // Post verisini ata
          this.comments = commentsData; // Yorum verisini ata
        })
        .catch((error) => {
          console.error("Error fetching data:", error); // Hata durumunda konsola yaz
        });
    },
    // Belirli bir post ID'sine ait yorumları filtrelemek için yardımcı metod
    filteredComments(postId) {
      return this.comments.filter(comment => comment.postId === postId);
    }
  },
};
</script>

<style>
#app {
  text-align: center;
  font-family: Arial, sans-serif;
  max-width: 800px; /* Maksimum genişlik */
  margin: 0 auto; /* Ortalar */
  padding: 20px; /* İç boşluk */
  background-color: #f8f9fa; /* Arka plan rengi */
  border-radius: 8px; /* Kenarları yuvarlat */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1); /* Gölge efekti */
}

h1 {
  color: #343a40; /* Başlık rengi */
  font-size: 2.5em; /* Font boyutu */
  margin-bottom: 20px; /* Alt boşluk */
}

h2 {
  color: #495057; /* Alt başlık rengi */
  font-size: 2em; /* Font boyutu */
  margin-top: 30px; /* Üst boşluk */
}
h3{
  color:rgb(245, 74, 74);
  font-size: 1.5em;
  margin-top: 20px;
}
h4 {
  color: #007bff; /* Yorum başlığı rengi */
  margin-top: 20px; /* Üst boşluk */
}

ul {
  list-style-type: none; /* Madde işaretlerini kaldır */
  padding: 0; /* İç boşluğu kaldır */
}

li {
  background-color: #ffffff; /* Liste öğesi arka plan rengi */
  border: 1px solid #dee2e6; /* Kenar rengi */
  border-radius: 5px; /* Kenarları yuvarlat */
  margin: 10px 0; /* Dikey boşluk */
  padding: 15px; /* İç boşluk */
  transition: box-shadow 0.2s; /* Geçiş efekti */
}

li:hover {
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2); /* Fare ile üzerindeyken gölge efekti */
}

strong {
  color: #212529; /* Kalın metin rengi */
}
</style>
