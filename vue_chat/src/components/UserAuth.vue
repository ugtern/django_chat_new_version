<template>
  <div class="container">
    <h1 class="text-center">Welcome!</h1>
    <div id="auth-container" class="row">
      <div class="col-sm-4 offset-sm-4">
        <ul class="nav nav-tabs nav-justified" id="myTab" role="tablist">
          <li class="nav-item">
            <a class="nav-link active" id="signup-tab" data-toggle="tab" href="#signup" role="tab" aria-controls="signup" aria-selected="true" @click="selected=1">Sign Up</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" id="signin-tab" data-toggle="tab" href="#signin" role="tab" aria-controls="signin" aria-selected="false" @click="selected=2">Sign In</a>
          </li>
        </ul>

        <div class="tab-content" id="myTabContent">

          <div class="tab-pane fade show active" id="signup" role="tabpanel" aria-labelledby="signin-tab" v-show="selected === 1">
            <form @submit.prevent="signUp">
              <div class="form-group">
                <input v-model="email" type="email" class="form-control" id="email" placeholder="Email Address" required>
              </div>
              <div class="form-row">
                <div class="form-group col-md-6">
                  <input v-model="username" type="text" class="form-control" placeholder="Username" required>
                </div>
                <div class="form-group col-md-6">
                  <input v-model="password" type="password" class="form-control" placeholder="Password" required>
                </div>
              </div>
              <div class="form-group">
                <div class="form-check">
                  <input class="form-check-input" type="checkbox" id="toc" required>
                  <label class="form-check-label" for="gridCheck">
                    Accept terms and Conditions
                  </label>
                </div>
              </div>
              <button type="submit" class="btn btn-block btn-primary">Sign up</button>
            </form>
          </div>

          <div class="tab-pane fade show active" id="signin" role="tabpanel" aria-labelledby="signin-tab" v-show="selected === 2">
            <form @submit.prevent="signIn">
              <div class="form-group">
                <input v-model="username" type="text" class="form-control" placeholder="Username" required>
              </div>
              <div class="form-group">
                <input v-model="password" type="password" class="form-control" placeholder="Password" required>
              </div>
              <button type="submit" class="btn btn-block btn-primary">Sign in</button>
            </form>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script>

  export default {

      data () {
        return {
            email: '',
            username: '',
            password: '',
            selected: 2,
        }
      },
      methods: {
        signUp () {
          $.post('http://localhost:8000/auth/users/', this.$data, (data) => {

            console.log('complete');
            alert("Your account has been created. You will be signed in automatically");
            this.signIn()
          })
          .fail((response) => {
            console.log('complete');
            alert(response.responseText)
          })
        },

        signIn () {
          const credentials = {username: this.username, password: this.password};


          $.post('http://localhost:8000/auth/token/login/', credentials, (data) => {
            sessionStorage.setItem('authToken', data.auth_token);
            sessionStorage.setItem('username', this.username);
            this.$router.push('/chats');
          })
          .fail((response) => {
            alert(response.responseText)
          })
        }
      }

  }
</script>

<style scoped>
  #auth-container {
    margin-top: 50px;
  }

  .tab-content {
    padding-top: 20px;
  }
</style>
