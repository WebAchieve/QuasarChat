<template>
<q-form @submit="submitForm">
  <q-input v-if="tab == 'register'" outlined v-model="formData.name" label="Имя"  class="q-mb-md"/>
  <q-input outlined v-model="formData.email" type="email" label="Email" class="q-mb-md" />
   <q-input outlined v-model="formData.password" type="password" label="Пароль от 6 символов" class="q-mb-md" />
   <q-btn color="primary" :label="tab" type="submit" />
</q-form>
</template>
<script >
import { mapActions } from 'vuex'
export default {
  props:['tab'],
  data(){
    return{
      formData:{
        name:'',
        email:'',
        password:''
      }
    }
  },
  methods:{
    ...mapActions('store',['registerUser','loginUser']),
    submitForm(){
      if(this.tab == 'login'){
        this.loginUser(this.formData)
      }else{
        this.registerUser(this.formData)
      }
    }
  }
}
</script>
