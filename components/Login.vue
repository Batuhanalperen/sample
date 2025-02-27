<template>
  <v-dialog v-model="isVisible" width="500" @click:outside="isVisible = false">
    <template #activator="{ on, attrs }">
      <div class="activator" v-bind="attrs" v-on="on">
        {{ $t('menu.login') }}
      </div>
    </template>

    <v-card>
      <v-card-title class="headline grey lighten-2">
        {{ $t('menu.login') }}
        <v-icon @click="isVisible = false"> mdi-close </v-icon>
      </v-card-title>
      <v-container class="login">
        <div class="lang">
          <SwitchLang />
        </div>
        <v-form ref="form" v-model="valid">
          <v-row>
            <v-col cols="12">
              <v-text-field
                v-model="user.name"
                :rules="nameRules"
                :label="$t('login.name')"
                required
              />
            </v-col>

            <v-col cols="12">
              <v-text-field
                v-model="user.email"
                :rules="emailRules"
                :label="$t('login.email')"
                required
              />
            </v-col>
            <v-col cols="12">
              <v-text-field
                v-model="user.password"
                :rules="passwordRules"
                :type="showPass ? 'text' : 'password'"
                :append-icon="showPass ? 'mdi-eye' : 'mdi-eye-off'"
                :label="$t('login.password')"
                required
                @click:append="showPass = !showPass"
              />
            </v-col>
          </v-row>
        </v-form>
      </v-container>

      <v-divider></v-divider>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="primary" text @click="handleLogin">
          {{ $t('menu.login') }}
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      isVisible: false,
      valid: false,
      showPass: false,
      user: {
        name: '',
        email: '',
        password: '',
      },
      nameRules: [
        (v) =>
          !!v || this.$t('login.required', { name: this.$t('login.name') }),
      ],
      emailRules: [
        (v) =>
          !!v || this.$t('login.required', { name: this.$t('login.email') }),
        (v) => /.+@.+/.test(v) || this.$t('login.mailValidator'),
      ],
      passwordRules: [
        (v) => /(?=.{8,})/.test(v) || this.$t('login.passValidator'),
      ],
    }
  },
  watch: {
    isVisible(isVisible) {
      if (isVisible && this.$refs.form) this.$refs.form.resetValidation()
    },
  },
  methods: {
    handleLogin() {
      this.$refs.form.validate()
      if (this.valid) {
        this.$store.commit('setUser', this.user)
        this.isVisible = false
      }
    },
  },
}
</script>
<style lang="scss" scoped>
.activator {
  display: flex;
  align-items: center;
  padding: 0 16px;
  height: 100%;
  transition: 0.2s all ease-in-out;
  &:hover {
    background: #cecece;
  }
}
.login {
  padding: 0 32px 32px 32px;
}
.headline {
  display: flex;
  justify-content: space-between;
}
.lang {
  width: 100%;
  margin: 16px 0;
  display: flex;
  justify-content: flex-end;
}
</style>
