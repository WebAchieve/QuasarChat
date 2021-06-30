<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar class="justify-between">
<q-btn v-if="$route.fullPath.includes('/chat')" to='/users' color="primary" icon="arrow_back" />

        <q-toolbar-title class="absolute-center">
         {{title()}}
        </q-toolbar-title>
<div v-if="!$route.fullPath.includes('/chat')">
  <q-btn  color="primary" icon="account_circle" label="Войти" to="/auth"
v-if="!userDetails.userId"

 />
 <q-btn  color="primary" icon="account_circle" label="Выйти "
v-else
@click="logoutUser"
 ></q-btn>
</div>

 <div class="user">
   {{userDetails.name}}
 </div>
      </q-toolbar>
    </q-header>


    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { mapState,mapActions }  from 'vuex'
import mixinOtherUserDetails from 'src/mixins/mixin-other-user-details.js'


export default {
  mixins:[mixinOtherUserDetails],
computed:{
  ...mapState('store',['userDetails'])
},
methods:{
...mapActions('store',['logoutUser']),
title(){
  if(this.$route.fullPath.includes('/chat')) return this.otheruserDetails.name
  else if(this.$route.fullPath == '/auth') return  "Вход"
  else if(this.$route.fullPath == '/') return  "MyChat"

}

},

}
</script>
<style scoped>
.user{
  font-size: 0.8rem;
}
</style>
