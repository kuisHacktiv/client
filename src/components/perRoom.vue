<template>
  <b-card class="text-center">
    <h4 class="border-bottom pb-2">{{ detailroom.roomname }}</h4>
    <div v-if="jumlahMember < 4" class="btn btn-primary my-2" @click="joinRoom">
      Join Room
    </div>
    <div v-else disabled>Join Room</div>
  </b-card>
</template>

<script>
import axios from 'axios'
export default {
  name: 'perroom',
  props: ['detailroom'],
  data() {
    return {
      detail: null,
      jumlahMember: null
    }
  },
  methods: {
    joinRoom() {
      // console.log(this.detailroom.id, '<<<<< ini det')
      let objJR = {
        id: this.detailroom.id,
        roomname: this.detailroom.roomname,
        userId: localStorage.getItem('userId'),
        username: localStorage.name
      }
      let roomnamenya = this.detailroom.roomname
      this.$store.dispatch('joinRoom', objJR).then(() => {
        this.$socket.emit('joinRoom', roomnamenya)
      })
      this.$router.push(`/${roomnamenya}/lobby`)
    },
    getRoomDetail() {
      // console.log(this.detailroom, "<<");
      let nameRoom = this.detailroom.roomname
      axios({
        method: 'GET',
        url: `${this.$baseUrl}/roomDetail/${nameRoom}`
      })
        .then(({ data }) => {
          // console.log(data, "<<< ini room detail");
          this.detail = data
          this.jumlahMember = data.length
        })
        .catch((err) => {
          console.log(err)
        })
    }
  },
  created() {
    this.getRoomDetail()
  },
  mounted() {}
}
</script>

<style></style>
