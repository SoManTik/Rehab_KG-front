<script setup>
import { ref, onMounted, computed, watch, nextTick } from 'vue'
import { RouterLink, useRoute, useLink, useRouter } from 'vue-router'

const router = useRouter();
const route = useRoute();

// credentials
const credentials = ref({
  email: '',
  pwd: ''
});

const login = async () => {

  // fetch
  const response = await fetch('/api/login', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(credentials.value)
  });

  // response
  const data = await response.json();

  if (!data.user) {
    alert(data.message)
  } else {
    // save token to local storage
    localStorage.setItem('token', data.token);
    router.push('/');
    alert("Welcome")
  }



}

</script>

<template>
  <main>

    <section class="vh-100" style="background-color: #05c1bc;">
      <div class="container py-5 h-100">
        <div class="row d-flex justify-content-center align-items-center h-100">
          <div class="col col-xl-10">
            <div class="card" style="border-radius: 1rem;">
              <div class="row g-0">
                <div class="col-md-7 col-lg-6 d-none d-md-block">
                  <img src="/login_picture.png" alt="login form" style="border-radius: 1rem 0 0 1rem;" width="100%"
                    height="100%" />
                </div>
                <div class="col-md-5 col-lg-6 d-flex align-items-center">
                  <div class="card-body p-4 p-lg-5 text-black">

                    <form>


                      <h5 class="fw-normal mb-3 pb-3" style="letter-spacing: 1px;">Sign into your account</h5>

                      <div class="form-outline mb-4">
                        <input v-model="credentials.email" required type="email" id="form2Example17"
                          class="form-control form-control-lg" />
                        <label class="form-label" for="form2Example17">Email address</label>
                      </div>

                      <div class="form-outline mb-4">
                        <input v-model="credentials.pwd" required type="password" id="form2Example27"
                          class="form-control form-control-lg" />
                        <label class="form-label" for="form2Example27">Password</label>
                      </div>

                      <div class="pt-1 mb-4">
                        <button class="btn btn-dark btn-lg btn-block" type="button" @click="login()">Login</button>
                      </div>

                      <a class="small text-muted" href="#!">Forgot password?</a>
                      <p class="mb-5 pb-lg-2" style="color: #393f81;">Don't have an account? <a href="#!"
                          style="color: #393f81;">Register here</a></p>
                      <a href="#!" class="small text-muted">Terms of use.</a>
                      <a href="#!" class="small text-muted">Privacy policy</a>
                    </form>

                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

  </main>
</template>
