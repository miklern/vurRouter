<template>
   <div class="wrapper">
      <Header>
      </Header>
      <UserList>
         <div class="content">
            <ProfileInfo v-if="selectedUserId !== null" :userId="selectedUserId" @onAlbumsIdSelect="onAlbumsIdSelectHandler"/>
         </div>
      </UserList>
      <Footer>
      </Footer>
   </div>
</template>

<script>

import ProfileInfo from "../components/ProfileInfo";
import UserList from '../layouts/UserList';
import Footer from '../layouts/Footer';
import Header from '../layouts/Header';

export default {
   props: ['id'],
   name: "Home",
   components: {
   ProfileInfo,
   UserList,
   Header,
   Footer,
   },
   data() {
      return {
         selectedUserId: null,
      };
   },
   async created() {
      await this.userIdInRouteProps();
   },
   methods: {
      async userIdInRouteProps() {
         if ( this.$route.params.id !== undefined) {
         this.selectedUserId = this.id;
         }
      },
      onUserIdSelectHandler(userId) {      
         this.selectedUserId = userId;
         this.$router.push({ name: 'User', params: { id: userId }}).catch(()=>{});
      },
      onAlbumsIdSelectHandler(albumsId) {
         this.$router.push({ name: 'Photos', params: { albumsId: albumsId }}).catch(()=>{});
      }
   },
   watch: {
      async '$route'( newValue, oldValue ) {
         if ( newValue !== oldValue ) {
            this.selectedUserId = newValue;
            await this.userIdInRouteProps();
         }
      },
      async selectedUserId(newValue, oldValue) {
         if ( newValue !== oldValue ) {
            await this.userIdInRouteProps();
         }
      }
   }
};
</script>
