<template>

<v-app>















            <v-flex xs12 >


<div  class="suggested">
  <v-list two-line class = "suggested-list">
            <template v-for="(item, index) in myMessages"  >
              <v-list-tile
                avatar
                ripple
                @click="toggle(index)"

              >
              <v-list-tile-avatar>
                <img :src="item.avatar">
              </v-list-tile-avatar>
                <v-list-tile-content>
                  <v-list-tile-title>{{ item.name }}</v-list-tile-title>
                  <v-list-tile-sub-title class="text--primary">{{ item.message }}</v-list-tile-sub-title>

                </v-list-tile-content>

              </v-list-tile>
              <v-divider v-if="index + 1 < myMessages.length" :key="index"></v-divider>
            </template>
          </v-list>

          <audio id="player" ref="player" value = "hidden" :src="file"></audio>
         </div>







  </v-flex>
 <v-layout row wrap style="transform: translateY(-40%)">
  <v-flex xs10>
    <textarea   v-model="message" @keyup.enter = "sendMessage"
  							rows="5"
  							style="width: 100%;
                border: none;
                height: 100%;"




  							placeholder="Type a message" >
  						</textarea>

  </v-flex>


  <v-flex xs2 >
    <button type = "button" style="width: 100%; height: 100%; background-color:#009688;">Send</button>

  </v-flex>

</v-layout>














</v-app>

</template>




<script>




import Vue from 'vue'
import axios from 'axios'
import {mapGetters} from 'vuex'

export default {


data(){


return {

  data: [],
    busy: false,

  items: [
           { header: 'Today' },
           { avatar: 'https://scontent.ficn2-1.fna.fbcdn.net/v/t1.0-1/p160x160/29468236_901369833374211_8734349036217171968_n.jpg?_nc_cat=0&oh=f8f7428a3e9e807d58b3ef91ef215062&oe=5B760837', title: 'Brunch this weekend?', subtitle: "  I'll be in your neighborhood doing errands this weekend. Do you want to hang out?", name: '' },
           { divider: true, inset: true },

         ],



         users: [
         { active: true, title: 'Priscilla', avatar: 'https://scontent.ficn2-1.fna.fbcdn.net/v/t1.0-9/26734343_1963222710598716_4444436793604184869_n.jpg?_nc_cat=0&oh=7d16dfb49268afd4c548d37a9f4a6f77&oe=5B373C24' },
         { active: true, title: 'Ranee Carlson', avatar: '/static/doc-images/lists/2.jpg' },
         { title: 'Cindy Baker', avatar: '/static/doc-images/lists/3.jpg' },
         { title: 'Ali Connors', avatar: '/static/doc-images/lists/4.jpg' },
         { active: true, title: 'Jason Oner', avatar: '/static/doc-images/lists/1.jpg' },
         { active: true, title: 'Ranee Carlson', avatar: '/static/doc-images/lists/2.jpg' },
         { title: 'Cindy Baker', avatar: '/static/doc-images/lists/3.jpg' },
         { title: 'Ali Connors', avatar: '/static/doc-images/lists/4.jpg' },
         { active: true, title: 'Jason Oner', avatar: '/static/doc-images/lists/1.jpg' },
         { active: true, title: 'Ranee Carlson', avatar: '/static/doc-images/lists/2.jpg' },
         { title: 'Cindy Baker', avatar: '/static/doc-images/lists/3.jpg' },
         { title: 'Ali Connors', avatar: '/static/doc-images/lists/4.jpg' },
         { active: true, title: 'Jason Oner', avatar: '/static/doc-images/lists/1.jpg' },
         { active: true, title: 'Ranee Carlson', avatar: '/static/doc-images/lists/2.jpg' },
         { title: 'Cindy Baker', avatar: '/static/doc-images/lists/3.jpg' },
         { title: 'Ali Connors', avatar: '/static/doc-images/lists/4.jpg' }
       ],
       users2: [
         { title: 'Travis Howard', avatar: '/static/doc-images/lists/5.jpg' }
       ],

       message: '',


        myMessages:{


          messages: [

            { header: 'Today' },
            { avatar: 'https://scontent.ficn2-1.fna.fbcdn.net/v/t1.0-1/p160x160/29468236_901369833374211_8734349036217171968_n.jpg?_nc_cat=0&oh=f8f7428a3e9e807d58b3ef91ef215062&oe=5B760837', title: 'Brunch this weekend?', subtitle: "  I'll be in your neighborhood doing errands this weekend. Do you want to hang out?" },
            { divider: true, inset: true }

          ]
        },







        myFriends:{

          friendLists: []
        },

        currentUser: '',
        secondUser: '',
        tempMessage: '',
        messageStorage: [],

        messageValue: 0,
        tempName: '',
        currentUserName: '',
        secondUserName: '',
        currentUserId : '',
        currentUserRole: '',
        initializeMessage: -20,
        isActive: true,
        file: undefined,
        audio: undefined,
        scrollValue: 20,
        max: false


}

},



watch:{




},

computed:mapGetters([

  'isLogged'
]),


mounted(){


var vm= this






  vm.$socket.on('sendMessage', function(data){
  vm.myMessages.push({'avatar' : 'https://scontent.ficn2-1.fna.fbcdn.net/v/t1.0-1/p160x160/29468236_901369833374211_8734349036217171968_n.jpg?_nc_cat=0&oh=f8f7428a3e9e807d58b3ef91ef215062&oe=5B760837', 'name': vm.secondUserName, 'message': data.message})
var myData = data.messagedata
var connected = false
var messageCount = 0;



for( var i=0; i<data.clientsData.length; i++){

  if(data.clientsData[i] === data.bonusdata ){
    connected = true
  axios.put(`/api/editMessage/${myData}`).then(function(response){


    vm.scrollToEnd()


  }).catch(function(error){

    console.log(error)
  })

  }
}





  }.bind(this))

  vm.$socket.on('messageNotification', function(data){
var vm = this



for(var i =0; i<vm.myFriends.length; i++){

  if(vm.myFriends[i]['id'] === data.bonusdata){

    Vue.set(vm.myFriends[i], 'notif', vm.myFriends[i]['notif'] +=1)

  }
}
  }.bind(this))

  vm.$socket.on('yourFriend', function(data){

    let myFriend = data



if(data !=null){

    for(var i =0; i<vm.myFriends.length; i++){





      if (vm.myFriends[i]['id'] === myFriend){



          Vue.set( vm.myFriends[i], 'isActive', true)


      }






    }

  }











  }.bind(this))



vm.$socket.on('Now', function(data){
  if(vm.isLogged){


  vm.socketOp = new FormData();

  vm.socketOp.append('socketId', vm.$socket.id)

  axios.post('api/initializeData', vm.socketOp).then(function(response){


  for(var i =0; i<vm.myFriends.length; i++){

    if(vm.myFriends[i]['id'] === data.currentUserId){

      Vue.set(vm.myFriends[i], 'current_conn_id', data.mySocket)
      Vue.set(vm.myFriends[i], 'previous_conn_id', data.mySocket)

      if(vm.getUserSock['id'] === data.currentUserId){

        Vue.set(vm.getUserSock, 'current_conn_id', data.mySocket)
        Vue.set(vm.getUserSock, 'previous_conn_id', data.mySocket)
      }

    }



  }




  }).catch(function(error){


  })
  }

}.bind(this))





vm.$socket.on('userDisconnected', function(data){

vm.friendLists()

  let myFriend = data





  for(var i =0; i<vm.myFriends.length; i++){





    if (vm.myFriends[i]['previous_conn_id'] === data ||  vm.myFriends[i]['current_conn_id'] === data){



        Vue.set( vm.myFriends[i], 'isActive', false)


    }






  }

}.bind(this))


},





beforeMount(){
var vm = this



   this.friendLists()

   var id  = vm.$route.params.friendId
   var chatroute = vm.$route.params.friendChatRoute
   var item  = vm.$route.params.friendItem
   var index = vm.$route.params.friendIndex

    vm.selectFriend(id, chatroute, item, index)












},


methods:{


getTestData(){

axios.get('api/testData').then(function(response){



}).catch(function(error){


  console.log(error)
})

},

limit( event, dataProp, limit ) {
           if ( this[dataProp].length >= limit ) {
              event.preventDefault();
           }

         },



initScroll(){
var vm = this

var container = vm.$el.querySelector('.suggested > .list ')
container.addEventListener("scroll", ()=>{

if(container.scrollTop === 0 && vm.max === false){


  vm.myFriend = new FormData();


  vm.myFriend.append('secondUser', vm.secondUser)
  vm.myFriend.append('scrollValue', vm.scrollValue)

  axios.post('/api/getMessages', vm.myFriend).then(function(response){


  vm.max = response.data.max

  console.log(response.data.scrollValue)




  Vue.set(vm.$data, 'myMessages', response.data.messages)


  vm.scrollValue+=20
  vm.currentUserName = response.data.currentUserName
  vm.secondUserName = response.data.secondUserName




  }).catch(function(error){

    console.log(error)
  })




}





})


},











sendMessage(){

var vm  = this

vm.tempMessage = vm.message
vm.myMessages.push({'avatar' : 'https://scontent.ficn2-1.fna.fbcdn.net/v/t1.0-1/p160x160/29468236_901369833374211_8734349036217171968_n.jpg?_nc_cat=0&oh=f8f7428a3e9e807d58b3ef91ef215062&oe=5B760837', 'name': vm.currentUserName, 'message': vm.message})

console.log(vm.message)





  vm.formData = new FormData();


  vm.formData.append('secondUser', vm.secondUser)
  vm.formData.append('message', vm.tempMessage)

  axios.post('api/saveMessage', vm.formData).then(function(response){



vm.$socket.emit('latestMessage', {message: vm.message, friend: vm.getUserSock, messagedata: response.data, bonusdata: vm.$socket.id, secondUser: vm.secondUser, myId: vm.currentUserId})

vm.scrollToEnd()
vm.message = ''


  }).catch(function(error){

    console.log(error)
  })


},



scrollToEnd () {
  var vm = this
        var container = vm.$el.querySelector('.suggested > .list ')
        container.scrollTop = container.scrollHeight


      },

      loadMore: function() {
      this.busy = true;

      setTimeout(() => {
        for (var i = 0, j = 10; i < j; i++) {
          this.data.push({ name: count++ });
        }
        this.busy = false;
      }, 1000);
    },







  async friendLists(){



var vm = this

if(vm.isLogged){


vm.$socket.connect()


}

     await axios.get('api/getFriendLists').then(function(response){
  let unreadMessages = response.data.unreadMessages
Vue.set(vm.$data, 'myFriends', response.data.friendLists)
for(var i =0; i<vm.myFriends.length; i++){
  Vue.set(vm.myFriends[i],'notif',unreadMessages[i+1])

}



vm.currentUserId = response.data.currentUserId
vm.currentUserRole = response.data.role

vm.$socket.emit('ImOn', {currentUserId: vm.currentUserId, mySocket: vm.$socket.id, friend: response.data.currentUser})

setInterval(function(){

vm.$socket.emit('friendOnline', vm.currentUserId)
}, 2000);


      }).catch(function(error){

        console.log(error)
      })



    },

     getUnreadMessages(){

      var vm = this

       axios.get('api/getUnreadMessages').then(function(response){
        let unreadMessages = response.data.allUnread


        for(var i =0; i<vm.myFriends.length; i++){
          Vue.set(vm.myFriends[i],'notif',unreadMessages[i+1])

        }



      }).catch(function(error){

        console.log(error)
      })

    },


    selectFriend(id, chatroute, item, index){

var vm = this
vm.max = false
vm.scrollValue = 20




vm.secondUser = id
vm.getUserSock = item

let currentUserId = vm.currentUserId
let role = vm.currentUserRole
vm.$socket.emit('roomdata', {firstUser: currentUserId.toString(), secondUser: id.toString(), currentRole: role, mySocketId: vm.$socket.id})


vm.myFriend = new FormData();


vm.myFriend.append('secondUser', id)
vm.myFriend.append('scrollValue', vm.scrollValue)

axios.post('/api/getMessages', vm.myFriend).then(function(response){




console.log(response.data.scrollValue)
  console.log('messages length', response.data.messages.length)
vm.max = response.data.max

Vue.set(vm.$data, 'myMessages', response.data.messages)


vm.scrollValue+=20
vm.currentUserName = response.data.currentUserName
vm.secondUserName = response.data.secondUserName


if(vm.myMessages){

Vue.set(vm.myFriends[index], 'notif', 0)

vm.scrollToEnd()


vm.initScroll()

}

}).catch(function(error){

  console.log(error)
})








    },




}


}





</script>

<style>


body{

  position: fixed;

  top: 0; right: 0; bottom: 0; left: 0;
}


.suggested > .list {
  background:#EEEEEE;

   overflow-y: auto;
height: calc(100vh - 9.5rem);

width: 100%;


}


.scrollable {
   overflow-y: auto;
   height: 100vh;

 }







</style>
