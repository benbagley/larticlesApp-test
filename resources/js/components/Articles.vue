<template>
  <div>
    <h1>Articles</h1>

    <!-- form for adding and editing articles -->
    <form class="w-full max-w-lg" @submit.prevent="addArticle">
      <div class="flex flex-wrap mb-6">
        <div class="w-full px-3 mb-6 md:mb-0">
          <label
            class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
            for="title"
          >Article title</label>
          <input
            class="appearance-none block w-full bg-gray-200 text-gray-700 border border-red-500 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white"
            id="title"
            type="text"
            placeholder="Article title"
            v-model="article.title"
          />
        </div>

        <div class="w-full px-3 mb-6 md:mb-0">
          <label
            class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
            for="body"
          >Article body</label>
          <textarea
            class="appearance-none block w-full bg-gray-200 text-gray-700 border border-red-500 rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white"
            id="body"
            placeholder="Article body"
            v-model="article.body"
          />
          <p class="text-gray-600 text-xs italic">Make it as long and as crazy as you'd like</p>
        </div>

        <div class="w-full px-3 mb-6 md:mb-0">
          <button
            class="w-full shadow bg-purple-500 hover:bg-purple-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded"
            type="submit"
          >Save</button>
        </div>
      </div>
    </form>
    <!-- end form for adding and editing articles -->

    <!-- articles -->
    <div class="flex">
      <article v-for="article in articles" v-bind:key="article.id">
        <section class="max-w-sm rounded overflow-hidden shadow-lg mx-2">
          <img
            class="w-full"
            src="https://images.unsplash.com/photo-1532562066470-f6f5f6d47924"
            alt="Owl"
          />
          <div class="px-6 py-4">
            <div class="font-bold text-xl mb-2">{{ article.title }}</div>
            <p class="text-gray-700 text-base">{{ article.body }}</p>
          </div>

          <hr />

          <div class="flex justify-center px-6 py-4">
            <button
              class="inline-block bg-blue-500 rounded-sm px-4 py-2 text-sm font-semibold text-white mr-2"
              @click="editArticle(article)"
            >Edit</button>
            <button
              class="inline-block bg-red-500 rounded-sm px-4 py-2 text-sm font-semibold text-white mr-2"
              @click="deleteArticle(article.id)"
            >Delete</button>
          </div>
        </section>
      </article>
    </div>
    <!-- end articles -->

    <!-- Pagination -->
    <section class="flex justify-center pt-12">
      <ul class="flex">
        <li
          v-bind:class="[{disabled: !pagination.previous_page_url}]"
          class="mx-1 px-3 py-2 bg-gray-200 text-gray-500 rounded-lg"
        >
          <a
            class="flex items-center font-bold"
            href="#"
            @click="fetchArticles(pagination.previous_page_url)"
          >
            <span class="mx-1">previous</span>
          </a>
        </li>
        <li
          class="mx-1 px-3 py-2 bg-gray-200 text-gray-700 hover:bg-gray-700 hover:text-gray-200 rounded-lg cursor-not-allowed"
        >
          <a class="font-bold" href="#">{{ pagination.current_page}} of {{pagination.last_page }}</a>
        </li>
        <li
          v-bind:class="[{disabled: !pagination.next_page_url}]"
          class="mx-1 px-3 py-2 bg-gray-200 text-gray-700 hover:bg-gray-700 hover:text-gray-200 rounded-lg"
        >
          <a
            class="flex items-center font-bold"
            href="#"
            @click="fetchArticles(pagination.next_page_url)"
          >
            <span class="mx-1">Next</span>
          </a>
        </li>
      </ul>
    </section>
    <!-- end pagination -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      articles: [],
      article: {
        id: "",
        title: "",
        body: ""
      },
      article_id: "",
      pagination: {},
      edit: false
    };
  },
  created() {
    this.fetchArticles();
  },
  methods: {
    fetchArticles(page_url) {
      let vm = this;
      page_url = page_url || "/api/articles";

      fetch(page_url)
        .then(res => res.json())
        .then(res => {
          this.articles = res.data;
          vm.makePagination(res.meta, res.links);
        })
        .catch(err => console.error(err));
    },
    makePagination(meta, links) {
      let pagination = {
        current_page: meta.current_page,
        last_page: meta.last_page,
        next_page_url: links.next,
        previous_page_url: links.prev
      };

      this.pagination = pagination;
    },
    addArticle() {
      if (this.edit === false) {
        // add an article
        fetch("/api/article", {
          method: "post",
          body: JSON.stringify(this.article),
          headers: {
            "content-type": "application/json"
          }
        })
          .then(res => res.json())
          .then(data => {
            this.article.title = "";
            this.article.body = "";
            alert("Article added");
            this.fetchArticles();
          })
          .catch(err => console.error(err));
      } else {
        // edit the article
        fetch("/api/article", {
          method: "put",
          body: JSON.stringify(this.article),
          headers: {
            "content-type": "application/json"
          }
        })
          .then(res => res.json())
          .then(data => {
            this.article.title = "";
            this.article.body = "";
            alert("Article updated");
            this.fetchArticles();
          })
          .catch(err => console.error(err));
      }
    },
    editArticle(article) {
      this.edit = true;
      this.article.id = article.id;
      this.article.article_id = article.id;
      this.article.title = article.title;
      this.article.body = article.body;
    },
    deleteArticle(id) {
      if (confirm("Are you sure")) {
        fetch(`/api/article/${id}`, {
          method: "delete"
        })
          .then(res => res.json())
          .then(data => {
            alert("Article removed");
            this.fetchArticles();
          })
          .catch(err => console.error(err));
      }
    }
  }
};
</script>
