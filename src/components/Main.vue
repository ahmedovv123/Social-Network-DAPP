<template>
  <div className="container-fluid mt-5">
    <div className="row">
      <main role="main" className="col-lg-12 ml-auto mr-auto" style="max-width:500px">
        <div className="content mr-auto ml-auto">
          <p> &nbsp; </p>
          <form @submit.prevent="submitForm">
            <div class="form-group mr-sm 2">
              <input 
                id="postContent" 
                type="text" 
                class="form-control" 
                placeholder="What`s on your mind ?" 
                v-model="content"
                required
              >
            </div>
            <button type="submit" class="btn btn-primary btn-block">Share</button>
          </form>
          <p> &nbsp; </p>
          <div v-for="(post,index) in posts" :key="index">
            <Post :post="post" :tipPost="tipPost" :index="index" />
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import Post from './Post.vue'

export default {
  name: 'Main',
  components: {
    Post
  },
  props: ['posts', 'createPost', 'tipPost'],
  setup(props) {

    const content = ref('')

    const submitForm = () => {
      props.createPost(content.value)
    }

    return { content, submitForm, }

  }

}
</script>

<style scoped>

</style> 