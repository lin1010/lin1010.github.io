<template>
  <div v-theme:column = "'wide'" id="show-blogs">
 <h1>博客总览</h1>
 <input type="text" placeholder="搜索" v-model="search">
 <div class="single-blog" v-for="(blog, index) in filteredBlogs" :key="index">
   <router-link :to="'/blog/'+ blog.id">
   <h2 v-rainbow>{{blog.title | to-uppercase}}</h2>
   </router-link>
   <article>
     {{blog.body | snippet}}
   </article>
 </div>
  </div>
</template>

<script>

import axios from 'axios'
export default {
  name: 'show-blogs',
  data () {
    return {
      blogs: [],
      search: ''
    }
  },
  created () {
    axios.get('/posts')
    // 请求本地文件
    // this.$http.get('./../static/posts.json')
      .then(data => {
        this.blogs = data.data.slice(0, 10)
      })
  },
  computed: {
    filteredBlogs () {
      return this.blogs.filter((blog) => {
        return blog.title.match(this.search)
      })
    }
  },
  filters: {
    toUppercase (value) {
      return value.toUpperCase()
    },
    snippet (value) {
      return value.slice(0, 100) + '...'
    }
  },
  directives: {
    'rainbow': {
      bind (el, binding, vnode) {
        el.style.color = '#' + Math.random().toString(16).slice(2, 8)
      }
    },
    'theme': {
      bind (el, binding, vnode) {
        if (binding.value === 'wide') {
          el.style.maxWidth = '1260px'
        } else if (binding.value === 'narrow') {
          el.style.maxWidth = '560px'
        }
        if (binding.arg === 'column') {
          el.style.background = '#6677cc'
          el.style.padding = '20px'
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#show-blogs{
  max-width: 800px;
  margin: 0 auto
}
.single-blog{
  padding: 20px;
  margin: 20px 0;
  box-sizing: border-box;
  background: #eee;
  border: 1px dotted #aaa
}

#show-blogs a{
  color: #444;
  text-decoration: none
}

input[type= 'text']{
  padding: 8px;
  width: 100%;
  box-sizing: border-box
}
</style>
