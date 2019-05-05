<template>
<div id="app">
  <div class = "container">
      <div class="row mt-4">
<div class = "col-md-6">
<section class="edit">
<h1>edit post</h1>
<form @submit.prevent="editPost()">
<div class="form-group">
<input class="form-control" type="text" name="title" id="title" placeholder="Title" v-model.trim="post.title"/>
</div>
<div class="form-group" rows="5" >
<textarea rows="5" class="form-control" type="text" name="description" id="description" placeholder="Description" v-model.trim="post.description" />
</div>
<div class="form-group">
<button type="submit" name="editPost">edit post</button>
</div>
</form>
<div>
<router-link :to="{ name: 'Posts' }">go to list of posts</router-link>
</div>
</section>
</div>

<div class = "col-md-6">
        <h4 class = "light">Preview</h4>
        <div class = "info" v-html='previewText'></div>
     </div>



</div>
</div>
</div>
</template>



<script>
import PostsService from '@/services/PostsService'
let marked = require('marked');
export default {
  name: "EditPostPage",
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
    return marked(this.post.description)
  }
  },
methods: {
      async getPost () {
        const response = await PostsService.getPost({ id: this.$route.params.id })
        this.post.title = response.data.title
        this.post.description = response.data.description
      },
      async editPost () {
        if (this.post.title !== '' && this.post.description !== '') {
          await PostsService.updatePost({
            id: this.$route.params.id,
            title: this.post.title,
            description: this.post.description
          })
          this.$router.push({ name: 'Posts' })
        }
      }
    },
    mounted () {
      this.getPost()
    }

};
</script>