<template>
  <div id="app">
    <HelloWorld msg="區塊鏈評分評價服務驗證" />
    <star-rating
      :increment="0.5"
      text-class="custom-text"
      @rating-selected="setRating"
    ></star-rating>
    <hr>
    <div v-html="ratingMsg"></div>
    <hr>
    先前評論總數: {{ lastNonce }} 
    <br>Transaction Details : <a :href="address" title="address" target="_blank">{{ address }}</a>
   
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import StarRating from "vue-star-rating";
import axios from "axios";

export default {
  name: "app",
  components: {
    HelloWorld,
    StarRating,
  },
  mounted () {
    this.getProduct()
  },
  data() {
    return {
      ratingMsg: "No Rating Selected",
      currentRating: "No Rating",
      currentSelectedRating: "No Current Rating",
      boundRating: 3,
      lastNonce: 0,
      newNonce: 0,
      address:'',
      showGetDataApi:''
    };
  },
  methods: {
    async setRatingAction(rating, getDetaCb) {
      const api =
        "https://tg-wallet-node.azurewebsites.net/poa/api/rating/product";
      const product = await axios
        .get(api, {
          params: {
            hash: '0x126e03e48bc4758d0df0cba481192d437abec3e550907f7f589a457016c5cdc0'
          }
        })
      const newNonce = product.data.data.nonce

      this.ratingMsg = "You have Selected: " + rating + " stars 請等待內容..."+ newNonce;
      this.lastNonce = newNonce - 1

      await this.postData(rating, newNonce)

      const delay = (interval) => {
          return new Promise((resolve) => {
              setTimeout(resolve, interval);
          });
      };

      await delay(3000);

      await getDetaCb(newNonce);     
    },

    async getProduct() {
      const api =
        "https://tg-wallet-node.azurewebsites.net/poa/api/rating/product";
      const product = await axios
        .get(api, {
          params: {
            hash: '0x126e03e48bc4758d0df0cba481192d437abec3e550907f7f589a457016c5cdc0'
          }
        })

      const newNonce = product.data.data.nonce
      this.lastNonce = newNonce - 1
    },
    async postData(rating, newNonce) {
      
      const postApi = 'https://tg-wallet-node.azurewebsites.net/poa/api/rating/comments'
      let res = await axios.post(postApi, {
        "userAddress":"0x500F5C0c90B1301777c30d2576eAF2cf53845606",
        "productHash":"0x126e03e48bc4758d0df0cba481192d437abec3e550907f7f589a457016c5cdc0",
        "star":rating*10,
        "comments":"add_star_"+newNonce
      })

      if(res.data)  {
        let transactionsAddr = res.data.data
        this.address = 'http://192.168.51.218/transactions/'+transactionsAddr+'/function'
      }
    },

    getDeta(newNonce) {
      const getDataApi = "https://tg-wallet-node.azurewebsites.net/poa/api/rating/comments?hash=0x126e03e48bc4758d0df0cba481192d437abec3e550907f7f589a457016c5cdc0&nonce="+newNonce
      this.showGetDataApi = getDataApi
      const api =
        "https://tg-wallet-node.azurewebsites.net/poa/api/rating/comments";
      axios
        .get(api, {
          params: {
            hash: '0x126e03e48bc4758d0df0cba481192d437abec3e550907f7f589a457016c5cdc0',
            nonce: newNonce 
          }
        })
      .then( this.handleGetDataSucc );
    },
    handleGetDataSucc(res) {
      console.log('handleGetDataSucc',res, this.newNonce)
      res = res.data
      const userAddress = res.data.userAddress
      const star = res.data.star / 10
      const comments = res.data.comments

      let msg = `UserID:${userAddress}  <br>\
              評論內容: ${comments} <br>\
              結果:${star}  顆星<br>
              ${this.showGetDataApi}<br> `;

      this.ratingMsg = msg 
    },
    reset() {
      this.ratingMsg = ''
      this.address = ''
      this.lastNonce = ''
    },
    setRating: function(rating) {
      this.reset()
      this.setRatingAction(rating, this.getDeta)
    },
    showCurrentRating: function(rating) {
      this.currentRating =
        rating === 0
          ? this.currentSelectedRating
          : "Click to select " + rating + " stars";
    },
    setCurrentSelectedRating: function(rating) {
      this.currentSelectedRating = "You have Selected: " + rating + " stars";
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}
.star {
  display: flex;
  justify-content: center;
}
.custom-text {
  font-weight: bold;
  font-size: 1.9em;
  border: 1px solid #cfcfcf;
  padding-left: 10px;
  padding-right: 10px;
  border-radius: 5px;
  color: #999;
  background: #fff;
}
</style>
