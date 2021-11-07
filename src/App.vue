<template>
  <div>
    <Navbar :account="account" />
    <div  class="text-center mt-5" v-if="loading" >
      <p >Loading...</p>
    </div>
    <Main v-else :posts="posts" :createPost="createPost" :tipPost="tipPost"/>
  </div>
</template>

<script>
import { onBeforeMount, ref } from 'vue'
import Navbar from './components/Navbar.vue'
import Main from './components/Main.vue'
import Web3 from 'web3'
import SocialNetwork from './abis/SocialNetwork.json'

export default {
  name: 'App',
  components: {
    Navbar,
    Main
  },
  setup(){

    const account = ref('')
    const socialNetwork = ref('')
    const postCount = ref('')
    const posts = ref([])
    const loading = ref(true)

    onBeforeMount( async () => {
      await loadWeb3()
      await loadBlockchainData()
      loading.value = false
    })

    const loadWeb3 = async () => {
      if(window.ethereum) {
        window.web3 = new Web3(window.ethereum)
        await window.ethereum.enable()
      }
      else if (window.web3) {
        window.web3 = new Web3(window.web3.currentProvider)
      }
      else {
        window.alert('Non-Ethereum browser detected. You should consider trying MetaMask!')
      }
    }

    const loadBlockchainData = async () => {
      const web3 = window.web3

      // Load account
      const accounts = await web3.eth.getAccounts()
      account.value = accounts[0]

      // Network ID
      const networkId = await web3.eth.net.getId()
      const networkData = SocialNetwork.networks[networkId]
      if(networkData) {
        socialNetwork.value = web3.eth.Contract(SocialNetwork.abi, networkData.address) 
        postCount.value = await socialNetwork.value.methods.postCount().call()

        // Load Posts
        for(let i = 1; i <= postCount.value; i++){
          const post = await socialNetwork.value.methods.posts(i).call()
          posts.value.push(post)

        }

        // Sort posts. Show highest tipped posts first
        posts.value = posts.value.sort((a,b) => b.tipAmount - a.tipAmount)

      } else {
        window.alert('SocialNetwork contract not deployed to detected network')
      }
    }

    const createPost = content => {
      loading.value = true
      socialNetwork.value.methods.createPost(content).send({ from: account.value })
      .on('transactionHash', (txHash) => {
        loading.value = false
      })
    }

    const tipPost = (id, tipAmount) => {
      loading.value = true
      socialNetwork.value.methods.tipPost(id).send( {from: account.value, value: tipAmount})
      .on('transactionHash', (txHash) => {
        loading.value = false
      })
    }

    return { account, posts, loading, createPost, tipPost }

  }
}
</script>

<style>

</style>
