<template>
  <div>
    <DinoModal :market="false" />
    <WalletModal v-show="isModalOpen" />
    <navbar />
    <section>
        <div class="index dinos">
            <div class="container">
                <div class="index-header">
                    <h3>MY DINOS</h3>
                </div>
                <dino-card :market="false" v-if="myDinos.length > 0" />
                <div class="no-data" v-if="myDinos.length === 0">
                    <h3>You don’t have any dinos :( <br>Let’s change that? :)</h3>
                </div>
            </div>
        </div>
    </section>
  </div>
</template>

<script>
import { mapActions, mapGetters, mapMutations, mapState } from 'vuex'
import { shortenAddress } from '../utils'
import DinoCard from '../components/DinoCard.vue'
import Navbar from '../components/Navbar.vue'
import DinoModal from './DinoModal.vue'
import WalletModal from '../components/WalletModal/index.vue'
import { connectors } from '../connectors'
export default {
  computed: mapState([
    'isModalOpen',
    'account',
    'wrongChainId',
    'totalSupply',
    'minting',
    'myDinos'
  ]),
  components: { Navbar, DinoCard, DinoModal, WalletModal },
  methods: {
    ...mapActions(['connectWallet', 'connectNetwork']),
    ...mapMutations(['openWalletModal', 'setMinting', 'AddTransaction']),
    ...mapGetters(['useMintContract']),
    shortenAddress
  },
  mounted () {
    this.$store.commit('CLEAR_FILTER', '')
    this.connectNetwork()
    this.connectWallet({ connector: connectors[0], first: 1 })
  }
}
</script>

<style>
.card-content .tag-list-item {
  padding:7px 5px
}

.no-data {
  color:#fff;
  text-align: center;
  font-size: 30px;
  line-height: 60px;
}

html {
  min-height: 100% !important;
    height: 100%;
}
</style>
