<template>
  <q-layout class="bg-blue-grey-1" view="lHh LPr Lff">
    <q-page-container>
      <q-page padding class="row items-center justify-center">
        <div class="row full-width">
          <div class="col-md-8 offset-md-2 col-xs-12 q-pa-md">
            <q-card flat class="text-blue-grey-14">
              <div class="row items-center">
                <div class="col-md-6 col-sm-12-col-xs-12">
                  <div class="row q-pt-md q-pb-md">
                    <div class="col-md-8 offset-2">
                      <q-img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ2_4sbjHYsJ4wUOkyCocH2h9Wz4w3d4AbpRA&usqp=CAU"></q-img>
                    </div>
                  </div>
                </div>
                <div class="col-md-6">
                  <q-card-section>
                    <div class="text-h4">Movie App</div>
                    <div>Login Akun Anda</div>
                  </q-card-section>
                  <q-form
                    @submit="login"
                  >
                    <q-card-section>
                      <q-input
                        v-model="username"
                        label="Username"
                        :rules="[
                          val => !!val || 'Tolong Isi Username Anda'
                        ]"
                      />
                      <q-input v-model="password" :type="isPwd ? 'password' : 'text'" label="Password"
                        :rules="[
                          val => !!val || 'Tolong Isi E-Mail Anda'
                        ]"
                      >
                        <template v-slot:append>
                          <q-icon
                            :name="isPwd ? 'visibility_off' : 'visibility'"
                            class="cursor-pointer"
                            @click="isPwd = !isPwd"
                          />
                        </template>
                      </q-input>
                    </q-card-section>
                    <q-card-section>
                      <q-btn
                        :loading="loading3"
                        :percentage="percentage3"
                        dark-percentage
                        unelevated
                        class="full-width"
                        color="primary"
                        @click="startComputing(3)"
                        label="Login"
                        type="submit"
                      />
                      <q-btn class="full-width q-mt-md" :to="{ name: 'registerPage' }" flat unelevated color="primary" label="Register" />
                    </q-card-section>
                  </q-form>
                </div>
              </div>
            </q-card>
          </div>
        </div>
      </q-page>
    </q-page-container>
  </q-layout>
</template>
<script>
export default {
  data () {
    return {
      username: null,
      password: null,
      isPwd: true,
      loading3: false,
      percentage3: 0
    }
  },
  methods: {
    login () {
      this.$axios.post('user/login', {
        username: this.username,
        password: this.password
      }).then(res => {
        if (res.data.sukses) {
          this.$q.localStorage.set('dataUser', res.data.data)
          if (res.data.data.level === 1) {
            this.$router.push({ name: 'dahsboardAdmin' })
          } else {
            this.$router.push({ name: 'homeUser' })
          }
        } else {
          this.$showNotif(res.data.pesan, 'negative')
        }
      })
    },
    startComputing (id) {
      this[`loading${id}`] = true
      this[`percentage${id}`] = 0
      this[`interval${id}`] = setInterval(() => {
        this[`percentage${id}`] += Math.floor(Math.random() * 8 + 10)
        if (this[`percentage${id}`] >= 100) {
          clearInterval(this[`interval${id}`])
          this[`loading${id}`] = false
        }
      }, 700)
    }
  },
  beforeDestroy () {
    clearInterval(this.interval1)
    clearInterval(this.interval2)
  }
}
</script>
