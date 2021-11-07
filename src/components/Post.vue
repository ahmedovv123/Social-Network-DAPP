<template>
<div class="card mb-4" :key="index" >
  <div class="card-header d-sm-flex">
    <div class='mr-2' v-html="getIcon(post.author)" ></div>
    <small class="text-muted mt-1">{{ post.author }}</small>
  </div>
  <ul id="postList" class="list-group list-group-flush">
    <li class="list-group-item" >
      <p>{{ post.content }}</p>
    </li>
    <li :key="index" class="list-group-item py-2">
      <small class="float-left mt-1 text-muted">TIPS: {{ toHuman(post.tipAmount.toString()) }} ETH </small> 
      <button @click="tipPost(post)" class="btn btn-link btn-sm float-right pt-0">
        <span>TIP 0.1 ETH</span>
      </button>
    </li>
  </ul>
</div>
</template>

<script>
import * as jdenticon from 'jdenticon'

export default {
  name: 'Post',
  props: ['post', 'index', 'tipPost'],

  setup(props) {

    const toHuman = amount => {
      return window.web3.utils.fromWei(amount, 'Ether')
    }

    const getIcon = address => {
      return jdenticon.toSvg(address, 30)
    }

    const tipPost = (post) => {
      let tipAmount = window.web3.utils.toWei('0.1', 'Ether')
      props.tipPost(post.id, tipAmount)
    }

    return { toHuman, getIcon, tipPost }
  }
}
</script>

<style scoped>

</style>