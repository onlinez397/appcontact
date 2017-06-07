<template >
<f7-page class="theme-cyan">
  <f7-navbar  title="Detail" back-link="Back" sliding></f7-navbar>
  <div id="profile_div">

  <p><img src="https://expertbeacon.com/sites/default/files/advice_for_men_on_selecting_your_online_dating_profile_photo.jpg" /></p>
  <p id="name">{{staff.display_name}}</p>
  <p id = "title">{{staff.employment_title}}</p>
  </div>




  <f7-block inner >



    <p><i class="icon icon-f7"></i> &nbsp  {{staff.display_name}}</p>
    <p>{{staff.display_chinesename}}</p>
    <p><i class="f7-icons email"></i> &nbsp {{staff.email}}</p>
    <p>{{staff.department_code}}</p>
    <p>{{staff.employment_title}}</p>


  </f7-block>



</f7-page>




</template>

<script>
import store from '../../../store.js'
	export default {
    data(){
    return{
        staff:store.selectedStaff

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
height: 50%;
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
/*
#profile_div img:before{
  display: block;
  	padding-top: 100%;
}*/



</style>
