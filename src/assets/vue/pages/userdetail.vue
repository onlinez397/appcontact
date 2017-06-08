<template >
<f7-page class="theme-cyan">
  <f7-navbar  title="Detail" back-link="Back" sliding></f7-navbar>
  <div id="profile_div">

  <p><img :src='staff.profile_image_src' /></p>
  <p id="name">{{staff.display_name}}</p>
  <p id = "title">{{staff.employment_title}}</p>
  </div>

  <f7-block inner >

    <!-- <p><a href="#" class="button" v-on:click="phoneCall(staff.phonenumber)">{{staff.phonenumber}}</a></p>
    <p><a href="#"  v-on:click="phoneCall(staff.phonenumber)">{{staff.phonenumber}}</a></p>
    <f7-list>
      <f7-list-item  icon="icon-bars" v-on:click="phoneCall(staff.phonenumber)" :title="staff.phonenumber"> </f7-list-item>
      <f7-list-item  title="Contacts"></f7-list-item>
    </f7-list>
    <p><i class="icon icon-f7"></i> &nbsp  {{staff.display_name}}</p>
    <p>{{staff.display_chinesename}}</p>
    <p><i class="f7-icons email"></i> &nbsp {{staff.email}}</p>
    <p>{{staff.department_code}}</p>
    <p>{{staff.employment_title}}</p>
    <p>This is {{staff.phonenumber}}</p> -->

    <div class="list-block">
      <ul>

        <li class="item-content" >

          <div class="item-media" ><i class="f7-icons">person_fill</i></div>
          <div class="item-inner" >
            <div class="item-title">{{staff.display_chinesename}}</div>

          </div>

        </li>
        <li class="item-content"  v-on:click="phoneCall(staff.phonenumber)">

          <div class="item-media" ><i class="f7-icons">phone_fill</i></div>
          <div class="item-inner" >
            <div class="item-title">{{staff.phonenumber}}</div>

          </div>

        </li>
        <li href="" class="item-content" v-on:click="sendMail(staff.email)" >

          <div class="item-media" ><i class="f7-icons">email_fill</i></div>
          <div class="item-inner" >
            <div class="item-title">{{staff.email}}</a></div>

          </div>

        </li>
        <li class="item-content" >

          <div class="item-media" ><i class="f7-icons">briefcase_fill</i></div>
          <div class="item-inner" >
            <div class="item-title">{{staff.department_fullname}}</div>

          </div>

        </li>
        <li class="item-content" >

          <div class="item-media" ><i class="f7-icons">navigation_fill</i></div>
          <div class="item-inner" >
            <div class="item-title">{{staff.room}}</div>

          </div>

        </li>

      </ul>

    </div>




  </f7-block>

  <f7-fab-speed-dial >

    <f7-fab-actions>
      <f7-fab-action color="green" @click="phoneCall(staff.phonenumber)"><i class="f7-icons">phone_fill</i></f7-fab-action>
      <f7-fab-action color="orange" @click="sendMail(staff.email)"><i class="f7-icons">email_fill</i></f7-fab-action>

    </f7-fab-actions>


    <f7-fab color="pink">

    <i class="fa fa-plus" aria-hidden="true"></i>

      <i class="fa fa-minus" aria-hidden="true"></i>

      </f7-fab>
  </f7-fab-speed-dial>

  <!-- <div class="speed-dial">

    <a href="#" class="floating-button">

      <i class="icon icon-plus"></i>

      <i class="icon icon-close"></i>
    </a>

    <div class="speed-dial-buttons">

      <a href="#" class="green" v-on:click="phoneCall(staff.phonenumber)">
        <i class="f7-icons">phone_fill</i>
      </a>

      <a href="#">
        <i class="icon demo-icon-calendar"></i>
      </a>

      <a href="#">
        <i class="icon demo-icon-upload"></i>
      </a>
    </div>
  </div> -->
</f7-page>




</template>

<script>
import store from '../../../store.js'
	export default {
    data(){
    return{
        staff:store.selectedStaff

    }
  },
  methods:{
    phoneCall:function(number){
      window.location.href = "tel:"+number

    },
    sendMail:function(email){
      window.location.href = "mailto:"+email
    }
  }
}
</script>
<style>

.navbar{
  /*background-color: yellow*/
}

#profile_div{
background: linear-gradient(-45deg, #479eaf, #56ad8b);
height: 45%;
text-align: center; margin: 1em 0;

}
#profile_div img{
border-radius: 100%;
width: 130px;
height:130px;

display: block;
margin: auto;
padding-top: 30px;
}

#profile_div p{
  line-height: 50%;
}


#name{
  font-size:x-large;
  color: white;
  font: arial bold;

}

#title{
  font-size:medium;
  color:black;
  font: arial italic;
/*font: italic bold arial,Georgia, serif;*/
}


.floating-button {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.speed-dial-buttons a{
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);

}
/*
#profile_div img:before{
  display: block;
  	padding-top: 100%;
}*/



</style>
