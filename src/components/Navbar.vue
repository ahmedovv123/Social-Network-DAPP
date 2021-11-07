<template>
  <nav className="navbar navbar-dark fixed-top bg-dark flex-md-nowrap p-0 shadow">
    <a
      className="navbar-brand col-sm-3 col-md-2 mr-0"
      href="http://www.dappuniversity.com/bootcamp"
      target="_blank"
      rel="noopener noreferrer"
    >
      Dapp University
    </a>
    <ul class="navbar-nav px-3">
      <li class="nav-item text-nowrap d-none d-sm-none d-sm-flex" >
        <small class="text-secondary mt-1 ">
          <small id="account">{{ account }}</small>
        </small>
        <div v-if="account" class="ml-2" v-html="icon" ></div>
        <span v-else></span>
      </li>
    </ul>
  </nav>
</template>

<script>
import * as jdenticon from 'jdenticon'

export default {
  name: 'Navbar',
  props: ['account'],
  setup(props) {

    const icon = jdenticon.toSvg(props.account.value, 30)

    return { icon }

  }
}
</script>

<style>

</style>