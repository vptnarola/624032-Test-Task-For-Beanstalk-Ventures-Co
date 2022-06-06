<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <div class="container">
        <q-toolbar>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
          </q-avatar>

          <q-toolbar-title> FinTech </q-toolbar-title>

            <div class="q-gutter-sm" style="width: auto" v-show="!isLogin">
              <q-btn color="white" text-color="black" label="Login"  @click="login = true" />
              <q-btn color="white" text-color="black" label="Sign Up"  @click="signUpModel = true" />
            </div>
            <div class="q-gutter-sm" style="width: auto" v-show="isLogin">
              <q-btn color="secondary" text-color="white" label="Add Property" @click="open('right')"/>
              <q-btn color="negative" text-color="white" label="Log Out" @click="logout()"/>
            </div>
        </q-toolbar>
      </div>
    </q-header>
    <div class="hero-section">
      <h1>Book Your Tour in Finland</h1>
    </div>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>

  <q-dialog v-model="login">
    <q-card style="width: 400px; max-width: 80vw;">
      <q-card-section>
        <div class="text-h6">Login</div>
      <q-banner inline-actions class="text-white bg-red" v-show="loginInfo.loginError">
        {{loginInfo.loginError}}
      </q-banner>
      </q-card-section>
      <q-card-section class="q-pt-none">
        <q-form class="q-gutter-md" @submit.prevent="varification()">
          <q-input filled type="email" label="Enter Email" v-model="loginInfo.email" :rules="[ val => val && val.length > 0 || 'Please Enter Email']"/>
          <q-input filled type="password" label="Enter Password" lazy-rules v-model="loginInfo.password" :rules="[ val => val && val.length > 0 || 'Please Enter Password']"/>
          <q-btn label="Login" type="submit" color="primary"/>
        </q-form>
      </q-card-section>
    </q-card>
  </q-dialog>

  <q-dialog v-model="signUpModel">
    <q-card style="width: 400px; max-width: 80vw;">
      <q-card-section>
        <div class="text-h6">Sign Up</div>      
      </q-card-section>
      <q-card-section class="q-pt-none">
        <q-form class="q-gutter-md" @submit.prevent="registration()">
          <q-input filled type="name" label="Enter Name" v-model="signUp.name" :rules="[ val => val && val.length > 0 || 'Please Enter Name']"/>
          <q-input filled type="email" label="Enter Email" v-model="signUp.email" :rules="[ val => val && val.length > 0 || 'Please Enter Email']"/>
          <q-input filled type="password" label="Enter Password" lazy-rules v-model="signUp.password" :rules="[ val => val && val.length > 6 || 'Please Enter More then 6 Character']"/>
          <q-btn label="Submit" type="submit" color="primary" />
        </q-form>
      </q-card-section>
    </q-card>
  </q-dialog>

  <q-dialog v-model="dialog" :position="position">
      <q-card class="full-height" style="width: 400px; max-width: 80vw;">
        <q-card-section>
          <div class="text-h6">Add Product</div>
        <q-banner inline-actions class="text-white bg-red" v-show="formData.propertyError">
          {{formData.propertyError}}
        </q-banner>
        </q-card-section>
        <q-card-section class="q-pt-none">
          <q-form class="q-gutter-md" @submit.prevent="submitHandler()">
            <q-input filled type="text" label="Enter Title" v-model="formData.title"  :rules="[ val => val && val.length > 0 || 'Please type something']"/>
            <q-input filled type="text" label="Enter Country" v-model="formData.address.country"  :rules="[ val => val && val.length > 0 || 'Please type something']"/>
            <q-input filled type="text" label="Enter city" v-model="formData.address.city"  :rules="[ val => val && val.length > 0 || 'Please type something']"/>
            <q-input filled type="number" label="Number of Bedrooms" v-model="formData.detail.bedrooms" :rules="[ val => val !== null && val !== '' || 'Please type valid Data', val => val > 0 && val < 100 || 'Please type valid Data' ]"/>
            <q-input filled type="number" label="Number of Bathroom" v-model="formData.detail.bathrooms" :rules="[ val => val !== null && val !== '' || 'Please type valid Data', val => val > 0 && val < 100 || 'Please type valid Data' ]"/>
            <q-input filled type="number" label="Number of Beds" v-model="formData.detail.beds" :rules="[ val => val !== null && val !== '' || 'Please type valid Data', val => val > 0 && val < 100 || 'Please type valid Data' ]"/>
            <q-input filled type="textarea" label="More Detail" v-model="formData.description"  :rules="[ val => val && val.length > 0 || 'Please type something']"/>
            <q-input @update:model-value="val => { formData.file = val[0] }" filled type="file" hint="Native file" />
            <q-btn label="Add" type="submit" color="primary"/>
          </q-form>
        </q-card-section>
      </q-card>
    </q-dialog>
</template>

<script>
import { defineComponent, ref } from 'vue'
// import EssentialLink from 'components/EssentialLink.vue'
import { api } from 'src/boot/axios'

export default defineComponent({
  name: 'MainLayout',
  components: {
    // EssentialLink
  },
  data(){
    return{
      formData:{
        title: "",
        description: "",
        address: {
          city: "",
          country: ""
        },
        detail: {
          bedrooms: 0,
          beds: 0,
          bathrooms: 0
        },
        file: null,
        propertyError: ""
      },
      signUp:{
        name: "",
        email: "",
        password: ""
      },
      loginInfo:{
        email: "",
        password: "",
        loginError: ""
      },
      isLogin: false
    }
  },
   setup () {
    const dialog = ref(false)
    const position = ref('top')
    return {
      login: ref(false),
      signUpModel: ref(false),
      dialog,
      position,
      open (pos) {
        position.value = pos
        dialog.value = true
      }
    }
  },
  methods:{
    submitHandler: function () {
      api.post('/property', this.formData).then(() => {
        window.alert('Property  Successfully Added')
        location.reload();
      }).catch(e => console.log(e))
      this.dialog = false
    },
    registration: function () {
      
        api.post('/user', this.signUp).then(()=> window.alert('Successfully Registerd')).catch(e => console.log(e))
        this.signUp.name = ''
        this.signUp.email = ''
        this.signUp.password = ''
        this.signUpModel = false
      
    },
    varification: function () {
        api.get('/user')
        .then((response)=> {
          const data = response.data;

          for (const key in data) {
            if (Object.hasOwnProperty.call(data, key)) {
              const element = data[key];
              if (this.loginInfo.email === element.email) {
                if (this.loginInfo.password === element.password) {
                  localStorage.setItem('isLogin', true)
                  localStorage.setItem('userId', element.id)
                  this.isLogin = true
                  this.loginInfo.loginError= ""
                  location.reload();
                  break;
                }
                else {
                  localStorage.removeItem('isLogin')
                  this.loginInfo.loginError= "Login Attempt Failed"
                }
                break;
              }
              else {
                localStorage.removeItem('isLogin')
                this.loginInfo.loginError = "Login Attempt Failed"
                console.log('lol')
              }
            }
          }
        })

    },
    logout: function () {
      localStorage.removeItem('isLogin')
      localStorage.removeItem('userId')
      location.reload();
      this.isLogin = false
    }
  },
  provide() {
    return {
      currentUser: this.isLogin
    }
  },
  mounted() {
    this.isLogin = localStorage.getItem('isLogin')
  }
})
</script>

<style>
.container{
  max-width: 1250px;
  padding: 0px 15px;
  margin: auto;
}
.hero-section{
  background-color: #4C3575;
  margin-top: 50px;
  padding: 60px 0px;
}
.hero-section h1{
  font-size: 35px;
  line-height: 1;
  color: #fff;
  text-align: center;
}

</style>
