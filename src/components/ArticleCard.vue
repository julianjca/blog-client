<template>
  <div class="card">
    <h2 @click="sendId">{{ blog.title }}</h2>
    <h3>{{ blog.user.name }}</h3>
    <p>{{ blog.body }}</p>
    <h3 v-if="userId===blog.user._id" @click="removeBlog(blog._id)" id="remove">delete</h3>
    <h3 v-if="userId===blog.user._id" id="update" @click="toUpdatePage">update</h3>
    <div class="socmedbutton">
      <a :href="shareUrl" class="twitter-share-button" data-show-count="false"><img src="http://goinkscape.com/wp-content/uploads/2015/07/twitter-logo-final.png"></a>
      <div class="fb-share-button" data-layout="button_count" data-size="small" data-mobile-iframe="true"><a target="_blank" :href="fbUrl" class="fb-xfbml-parse-ignore"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c2/F_icon.svg/2000px-F_icon.svg.png"></a></div>
      </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'ArticleCard',
  props: ['blog', 'userId'],
  data () {
    return {
      id: this.blog._id,
      idUser: '',
      shareUrl: '',
      fbUrl: ''
    }
  },
  methods: {
    sendId () {
      this.$emit('openingBlog', this.id)
    },
    toUpdatePage () {
      this.$router.push(`/update/${this.id}`)
    },
    removeBlog (id) {
      let self = this
      axios({
        method: 'delete',
        url: `http://35.240.195.45/articles/${id}`,
        headers: {
          token: localStorage.getItem('token')
        }
      })
        .then(response => {
          setTimeout(() => {
            self.$emit('requestNewData', id)
          }, 500)
        })
        .catch(err => {
          console.log(err)
        })
    }
  },
  watch: {
    userId () {
      this.idUser = this.userId
    }
  },
  created () {
    this.shareUrl = `https://twitter.com/intent/tweet?text=http://blog1.juliananderson.xyz/blog/${
      this.blog._id
    }`
    this.fbUrl = `https://www.facebook.com/sharer/sharer.php?u=http://blog1.juliananderson.xyz/blog/${
      this.blog._id
    }`
  }
}
</script>

<style scoped>
.card {
  width: 600px;
  min-height: 200px;
  border: 2px solid black;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 50px;
}

h2 {
  cursor: pointer;
}

#update {
  margin-left: auto;
  margin-right: auto;
  text-align: center;
  width: 30%;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
  color: white;
  background-color: green;
}

#remove {
  margin-left: auto;
  margin-right: auto;
  text-align: center;
  width: 30%;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
  color: white;
  background-color: red;
}

.socmedbutton {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
}

.socmedbutton img {
  width: 30px;
  height: auto;
  margin-left: 20px;
}
</style>
