<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <q-page class="flex flex-center">
        <q-form
          @submit="onSubmit"
          class="q-gutter-md"
        >
          <q-input
            filled
            v-model="name"
            label="用户名"
            lazy-rules
            :rules="[ val => val && val.length > 0 || '用户名不能为空']"
          />

          <q-input
            filled
            type="password"
            v-model="password"
            label="密码"
            lazy-rules
            :rules="[
          val => val !== null && val !== '' || '密码不能为空'
        ]"
          />

          <div>
            <q-btn label="登陆" type="submit" color="primary"/>
            {{ info }}
          </div>
        </q-form>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import axios from 'axios'
import { Cookies } from 'quasar'

export default {
  data () {
    return {
      name: null,
      password: null,
      info: null
    }
  },

  methods: {
    onSubmit () {
      console.log(this.name)
      console.log(this.password)

      const api = `http://dkh.nmgldjy.com:81/service.asmx/UserLoginStr?name=${this.name}&psw=${this.password}`
      axios.get(api).then(res => {
        if (res.data === -3) {
          this.$q.notify({
            color: 'red-4',
            textColor: 'white',
            icon: 'cloud_done',
            message: '用户或密码错误'
          })
        } else {
          // console.log(res.data)
          Cookies.set('user_token', res.data)
          Cookies.set('user_name', this.name)
          this.$q.notify({
            color: 'green-4',
            textColor: 'white',
            icon: 'cloud_done',
            message: '登陆成功'
          })
          this.$router.push('/')
        }
      })
    }
  }
}
</script>
