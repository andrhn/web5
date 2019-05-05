<template>
<div id="app">
  <div class = "container">
    <div class="row mt-4">

        <div class = "col-md-6">
        <form>
        <div class="form-group">
        <input class="form-control" type="text" name="title" id="title" placeholder="Title" v-model.trim="post.title"/>
        </div>
        <div class="form-group">
        <textarea class="form-control" type="text" rows="5" name="description" id="description" placeholder="Description" v-model.trim="post.description" v-model = 'md_text'>
        </textarea>
        </div>
        <div class="form-group">
        <button class="btn btn-block btn-primary" type="button" name="addPost" id="addPost" @click="addPost()">add new post</button>
      </div>
        </form>
</div>
        <div class = "col-md-6">
                <h4 class = "light">Preview</h4>
                <div class = "info" v-html='previewText'></div>
             </div>
          <section>
              <button class="btn btn-success btn-block" type="button" @click="goBack()">go to posts page</button>
          </section>
    </div>
</div>
</div>

</template>

<script>
import PostsService from '@/services/PostsService'
let marked = require('marked');
export default {
  name: "NewPostPage",
  data () {
  return {
      post: {
        title: "",
        description: ""
      },
      md_text : '#### hello world ',
    };
  } ,
    computed: {
  previewText() {
      marked.setOptions({
      renderer: new marked.Renderer(),
      gfm: true,
      tables: true,
      breaks: true,
      pedantic: false,
      sanitize: true,
      smartLists: true,
      smartypants: false
    });
    return marked(this.md_text)
  }
  },
    methods: {
    async addPost() {
      if (this.post.title !== "" && this.post.description !== "") {
        await PostsService.addNewPost({
          title: this.post.title,
          description: this.post.description
        });

        this.$router.push({ name: "Posts" });

      } else {
        alert("Empty fields!");
      }
    },
    goBack() {
      this.$router.push({ name: "Posts" });
    }
  }
};
</script>