<template>
  <b-container class="d-flex flex-column flex-wrap justify-content-center align-content-center container_full-heigt">
    <b-form-group
      label-cols-lg="3"
      label="欢迎"
      label-size="lg"
      label-class="font-weight-bold pt-0"
      class="mb-0 div-border p-5"
    >
      <!-- Enter email -->
      <b-form-group label-cols-sm="4" label="Email:" label-align-sm="right" label-for="Email">
        <b-form-input
          v-focus
          trim
          type="email"
          id="Email"
          spellcheck="false"
          :state="emailState"
          v-model="email"
          placeholder="Email address"
        ></b-form-input>
      </b-form-group>
      <!-- enter password -->
      <b-form-group label-cols-sm="4" label="Password:" label-align-sm="right" label-for="Password">
        <b-form-input
          trim
          :state="passwordState"
          type="password"
          id="Password"
          placeholder="Enter your password"
          v-model="password"
        ></b-form-input>
      </b-form-group>
      <!-- submit btn -->
      <b-form-group label-cols-sm="4" label-align-sm="right" class="mb-0">
        <b-button
          class="btn-block"
          :disabled="!fulfill"
          @click="login"
          variant="outline-dark"
        >Log in</b-button>
      </b-form-group>
    </b-form-group>
  </b-container>
</template>

<script>
import isEmail from 'validator/es/lib/isEmail'

export default {
  name: 'Login',
  methods: {
    async login () {
      const vm = this
      try {
        const { data: res } = await vm.axios({
          url: '/session',
          method: 'POST',
          data: {
            email: vm.email,
            password: vm.password
          }
        })
        if (res.code === 1) {
          vm.$fm.success(`Good ${res.msg}`)
          vm.$store.commit('setUserName', res.userName)
          vm.$router.back()
        } else {
          vm.$fm.error(`Fail to login: ${res.msg}`)
        }
      } catch (error) {
        vm.$log.error(error)
        vm.$fm.NETERR()
      }
    }
  },
  computed: {
    passwordState () {
      const l = this.password.length
      if (l === 0) {
        return null
      }
      return l >= 6 && l <= 18
    },
    emailState () {
      if (this.email.length === 0) {
        return null
      }
      return isEmail(this.email)
    },
    fulfill () {
      const vm = this
      return vm.passwordState && vm.emailState
    }
  },
  data () {
    return {
      email: 'test@typeof.fun',
      password: 'test@typeof.fun'
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  }
}
</script>

<style lang="scss">
@import "@/styles/mixin.scss";
.container_full-heigt {
  @include full-heigt;
}
.home-title {
  text-align: center;
  @include bold-outline;
}
.div-border {
  @include bold-outline;
}
</style>
