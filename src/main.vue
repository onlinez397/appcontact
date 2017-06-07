<template>
<!-- App -->
<div id="app">

  <!-- Statusbar -->
  <f7-statusbar></f7-statusbar>




  <!-- Main Views -->
  <f7-views>
    <f7-view id="main-view" navbar-through :dynamic-navbar="true" main>
      <!-- Navbar -->
      <f7-navbar>
        <f7-nav-left>
          <f7-link icon="icon-bars" open-panel="left"></f7-link>
        </f7-nav-left>
        <f7-nav-center sliding>HSMC</f7-nav-center>
        <f7-nav-right>
          <f7-link icon="icon-bars" open-panel="right"></f7-link>

          </f7-link>
        </f7-nav-right>

      </f7-navbar>

      <!-- Pages -->

      <f7-pages>
        <f7-page>

          <f7-searchbar cancel-link="Cancel" placeholder="Search contacts" :clear="true" v-model="filterName"></f7-searchbar>
<p><a href="#" class="button" v-on:click="phoneCall('110')">Usual Button 1</a></p>
          <!-- <f7-icon icon="icon-home"></f7-icon> -->
        <i class="f7-icons">download_round_fill</i>
          <f7-list form>
            <!-- Enables Smart Select behavior by adding "smart-select" prop -->
            <f7-list-item smart-select title="Department" smart-select-open-in="picker">
              <!-- Select with values inside -->
              <select name="fruits" v-model="searchdept">
									  <option value="" selected>Please select department ...</option>
									<option v-for="department in departments" :value="department.fullname">{{department.fullname}}</option>

								</select>
            </f7-list-item>
            <!-- Multiple Smart Select With Searchbar -->
             <span  v-show="loading" class="preloader"></span>
          </f7-list>

          <f7-list　 class="hsmcstaff" media-list="">

            <f7-list-item link="/userdetail/" v-on:click="onClick(staff)" v-for="staff in filteredDepartments" media='<img src ="https://expertbeacon.com/sites/default/files/advice_for_men_on_selecting_your_online_dating_profile_photo.jpg">' :title="staff.display_name"
              :subtitle="staff.department_fullname"  :text="staff.phonenumber"></f7-list-item>

            </f7-list>


        </f7-page>

      </f7-pages>
    </f7-view>
  </f7-views>



</div>
</template>

<script>
import store from './store.js'


let self;

function addData(staffs) {
  for (let staff of staffs) {
    staff.phonenumber = '39635'+staff.ext;
    staff.profile_image_url = '<img src="https://ecampus.hsmc.edu.hk/moodle/myhsmc/staff/staffpicture.php?id=' + staff.id + '">';
    staff.mailto = 'maillto:'+staff.email;
    // 'https://ecampus.hsmc.edu.hk/moodle/myhsmc/staff/staffpicture.php?id='+staff.id
  }
}
export default {
  data() {
    return {
      staffs: [],
      staff: '',
      filterName: '',
      departments: [],
      department: '',
      searchdept: '',
      loading: false,
      number:'',
      mailto:''

    }
  },

  methods: {
    onClick: function(staff) {
      store.selectedStaff = staff;

    },
    phoneCall:function(number){
      window.location.href = "tel:"+number

    }
  },
  created() {
    self = this;

    this.$http.post('http://mcampus.hsmc.edu.hk/cyrus/test_contact.php')
      .then(response => {

  //  this.loading = true;

        let result = response.data[0].Rows;
        store.staffs = result;
        self.$data.staffs = result;
        addData(self.staffs);
        // this.loading = false;
      }, {
        headers: {
          'X-CSRF-Token': undefined
        }
      })


    this.$http.post('http://mcampus.hsmc.edu.hk/cyrus/test_contact_v2.php')
      .then(response => {
        let result = response.data[0].Rows;
        self.$data.departments = result;


      }, {
        headers: {
          'X-CSRF-Token': undefined
        }
      })


    // addData(self.staffs);


  },


  computed: {
    filteredStaffs() {
      return this.staffs.filter((element) => {
        return element.display_name.match(this.filterName);
      });

    },
    filteredDepartments() {
      return this.filteredStaffs.filter((element) => {
        return element.department_fullname.match(this.searchdept)
      })
    }
  }


}
</script>
<style>
.hsmcstaff .item-media img {
  border-radius: 100%;
  height: 80px;
  width: 80px;
  background-size: cover;
}


</style>
