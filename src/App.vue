<template>
<div id="app">
  <div class = "contaier">
  	<div class="row mt-4">
 		 <div class = "col-md-6">
  			<h4 class = "light">Markdown</h4>
  			<textarea class ="info"
  					v-model = 'md_text'></textarea>
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
let marked = require('marked');
export default {
  name: "app",
  data () {

  return {
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
};
</script>

<style scoped>
textarea {
	resize: none;
	border: 2px dashed yellow;
	outline: none;
}

.info {
	height: 400px;
	width: 100%;
	background-color: rgba(255, 255, 255, 0.9);
}

</style>
