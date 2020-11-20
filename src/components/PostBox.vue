<template>
  <div>
    <b-navbar type="dark" variant="dark">
      <b-navbar-nav>
        <b-navbar-brand href="#" center>
          <h1>HTTP Client</h1>
        </b-navbar-brand>
      </b-navbar-nav>
    </b-navbar>
    <b-container class="pt-3">
      <b-row>
        <b-col>
          <b-card no-body>
            <template #header>
              <h3 class="mb-0">Posts</h3>
            </template>
            <b-card-body
              id="nav-scroller"
              ref="content"
              style="position:relative; height:500px; overflow-y:scroll;"
              class="p-0"
            >
              <b-card
                v-for="(post, key) in posts"
                body-class="px-0 pb-0"
                class="m-1"
                :key="post.id + key"
                border-variant="success"
                align="center"
              >
                <b-card-sub-title class="mb-2"
                  >Jemand schrieb:</b-card-sub-title
                >
                <b-card-text>{{ post.content }}</b-card-text>
                <b-card-footer class="p-0"> #{{ post.id }} </b-card-footer>
              </b-card>
            </b-card-body>
          </b-card>
        </b-col>
        <b-col>
          <b-form-input
            class="mb-2"
            v-model="url"
            placeholder="URL eingeben"
          ></b-form-input>
          <b-form-input
            class="mb-2"
            v-model="newPost"
            placeholder="Kommentar eingeben"
          ></b-form-input>
          <b-button variant="success" @click="getRequest">GET</b-button>
          <b-button variant="dark" @click="postRequest">POST</b-button>
          <b-form-file
            v-model="file1"
            :state="Boolean(file1)"
            placeholder="Choose a file or drop it here..."
            drop-placeholder="Drop file here..."
          ></b-form-file>
          <b-button variant="dark" @click="postFile">Send File</b-button>
        </b-col>
      </b-row>
      {{ $data }}
    </b-container>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "PostBox",
  props: {
    msg: String,
  },
  data: function() {
    return {
      posts: "",
      url: process.env.test,
      newPost: "",
      file1: "",
    };
  },
  methods: {
    addFiles() {
      this.$refs.files.click();
    },
    getRequest() {
      axios
        .get(this.url, {
          withCredentials: true,
        })
        .then((response) => {
          this.posts = response.data;
          this.posts.reverse();
        });
    },
    postFile() {
      let formData = new FormData();
      formData.append("file1", this.file1);
      axios
        .post(this.url, formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        })
        .then((response) => {
          console.log(response);
        });
    },
    postRequest() {
      axios
        .post(
          this.url,
          {
            content: this.newPost,
          },
          {
            withCredentials: true,
          }
        )
        .then((response) => {
          this.posts = response.data;
          this.posts.reverse();
          this.newPost = "";
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    putRequest() {
      axios
        .put(
          this.url,
          {
            content: this.newPost,
          },
          {
            withCredentials: true,
          }
        )
        .then((response) => {
          this.posts = response.data;
          this.posts.reverse();
          this.newPost = "";
        });
    },
  },
  mounted() {
    this.getRequest();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
