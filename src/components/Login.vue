<template>
  <div class="columns">
    <div class="column is-offset-2 is-two-thirds">
      <article class="message is-primary">
        <div class="message-header">
          <p>Login</p>
        </div>
        <div class="message-body">
          <form @submit.prevent="onSubmit">
            <div class="is-size-5 has-text-danger is-bold has-text-centered" v-if="error">
              Invalid Username or password.
            </div>
            <div class="field">
              <label class="label">Username</label>
              <div class="control has-icons-right">
                <input :class="{'input': true, 'is-danger': errors.has('username'), 'is-success': !errors.has('username') && fields.username && fields.username.touched }"
                       type="text" name="username" placeholder="Username" v-model="user.username" v-validate="'required'">
                <span class="icon is-small is-right" v-show="!errors.has('username') && fields.username && fields.username.touched">
                  <i class="fa fa-check"></i>
                </span>
              </div>
              <p v-show="errors.has('username')" class="help is-danger">{{ errors.first('username') }}</p>
            </div>

            <div class="field">
              <label class="label">Password</label>
              <div class="control has-icons-right">
                <input :class="{'input': true, 'is-danger': errors.has('password'), 'is-success': !errors.has('password') && fields.password && fields.password.touched }"
                       type="password" name="password" placeholder="Password" v-model="user.password" v-validate="'required'">
                <span class="icon is-small is-right" v-show="!errors.has('password') && fields.password && fields.password.touched">
                  <i class="fa fa-check"></i>
                </span>
              </div>
              <p v-show="errors.has('password')" class="help is-danger">{{ errors.first('password') }}</p>
            </div>
            <div class="field is-pulled-right">
              <router-link :to="{ name: 'register'}" class="button is-default is-link">Register</router-link>
            </div>
            <div class="field">
              <button class="button is-primary" :disabled="errors.any()">Submit</button>
              <router-link :to="{ name: 'articles'}" class="button is-default is-outlined">Cancel</router-link>
            </div>


          </form>
        </div>
      </article>
    </div>
  </div>
</template>

<script>
import Auth from '../services/Auth';
import * as types from '../store/mutation-types';

export default {
  name: 'VdLogin',
  data () {
    return {
      redirect: '/',
      error: null,
      user: {
        username: '',
        password: ''
      }
    };
  },
  beforeRouteEnter (to, from, next) {
    next(vm => { vm.redirect = from.path; });
  },
  methods: {
    onSubmit () {
      this.error = false;

      Auth.login(this.user).then((response) => {
        this.$store.commit(types.SET_USER, {user: response.user});
        if (this.redirect.startsWith('/login')) {
          this.redirect = '/';
        }

        this.$router.push(this.redirect);
      }).catch(() => {
        this.error = true;
      });
    }
  }
};
</script>


<style scoped>

</style>
