<template>
  <div v-if="account">
    {{ account }}
    <button @click="logoutHandler">로그아웃</button>
  </div>
  <div v-else>
    <input v-model="form.userid">
    <input v-model="form.userpw">
    <button @click="loginHandler">로그인</button>
  </div>
</template>
<script>
  import axios from 'axios'

  export default {
    data() {
      return {
        form: {userid: '', userpw: ''}
      }
    },
    computed: {
      account() {
        return this.$store.state.user.userid
      }
    },
    created() {
      axios.get('/api/account')
      .then(result => {
        this.$store.commit('user', result.data)
      })
      .catch(() => {console.log('로그인 필요')})
    },
    methods: {
      loginHandler() {
        axios.post('/api/login', this.form)
        .then((result) => {
          this.$store.commit('user', result.data)
          alert('로그인 성공')
        })
        .catch((err) => {
          console.log(err)
          alert('로그인 실패')
        })
      },
      logoutHandler() {
        axios.post('/api/logout')
        .then(() => {
          this.$store.commit('user', {})
          alert('로그아웃 성공')
        })
        .catch((err) => {
          console.log(err)
          alert('로그아웃 실패')
        })
      }
    }
  }
</script>