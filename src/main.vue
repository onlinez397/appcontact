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
          <!-- <f7-list media-list>
  <f7-list-item
    link="/item/"
    media="<img src='http://mcampus.hsmc.edu.hk/sumiin/staff/staffImage.php?id=96&amp;thumb=1'>"
    title="Item Title"
    subtitle="Item Subtitle"
    text="Some text"
    after="Read more"
  ></f7-list-item>
</f7-list> -->

          <f7-list form>
            <!-- Enables Smart Select behavior by adding "smart-select" prop -->
            <f7-list-item smart-select title="Department" smart-select-open-in="picker">
              <!-- Select with values inside -->
              <select name="fruits" v-model="searchdept" @change="changeFilter()">
									  <option value="" selected>Please select department ...</option>
									<option v-for="department in departments" :value="department.fullname">{{department.fullname}}</option>

								</select>
            </f7-list-item>
            <!-- Multiple Smart Select With Searchbar -->
             <span  v-show="loading" class="preloader"></span>
          </f7-list>

          <f7-list　 class="hsmcstaff list_wrapper" media-list="" >

            <!-- <f7-list-item link="/userdetail/" v-on:click="onClick(staff)" v-for="staff in filteredDepartments" media='<img src ="https://expertbeacon.com/sites/default/files/advice_for_men_on_selecting_your_online_dating_profile_photo.jpg">' :title="staff.display_name"
              :subtitle="staff.department_fullname"  :text="staff.phonenumber"></f7-list-item> -->

              <f7-list-item link="/userdetail/" v-on:click="onClick(staff)" v-for="staff in filteredDepartments" :media="staff.profile_image_url" :title="staff.display_name"
                :subtitle="staff.department_fullname"  :text="staff.phonenumber" ></f7-list-item>
                <infinite-loading :on-infinite="onInfinite" :distance="distance" ref="infiniteLoading" spinner="bubble"></infinite-loading>
                </li>

            </f7-list>


        </f7-page>

      </f7-pages>
    </f7-view>
  </f7-views>



</div>
</template>

<script>
import store from './store.js';
import InfiniteLoading from 'vue-infinite-loading';
const api = 'http://mcampus.hsmc.edu.hk/cyrus/test_contact.php';

let self;

function addData(staffs) {
  for (let staff of staffs) {
    staff.phonenumber = '39635'+staff.ext;
    staff.profile_image_src = 'http://mcampus.hsmc.edu.hk/sumiin/staff/staffImage.php?id='+staff.id+'&amp;thumb=1';
    staff.profile_image_url = '<img src="http://mcampus.hsmc.edu.hk/sumiin/staff/staffImage.php?id='+staff.id+'&amp;thumb=1">';
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
      mailto:'',
      list:[],
      tag: 'story',

    }
  },

  methods: {

    //
    // onInfinite() {
    //   this.$http.get(api, {
    //     params: {
    //       tags: this.tag,
    //       page: this.list.length / 20 + 1,
    //     },
    //   }).then((res) => {
    //     if (res.data.hits.length) {
    //       this.list = this.list.concat(res.data.hits);
    //       this.$refs.infiniteLoading.$emit('$InfiniteLoading:loaded');
    //       if (this.list.length / 20 === 10) {
    //         this.$refs.infiniteLoading.$emit('$InfiniteLoading:complete');
    //       }
    //     } else {
    //       this.$refs.infiniteLoading.$emit('$InfiniteLoading:complete');
    //     }
    //   });
    // },


    onInfinite() {
     this.$http.get(api, {
       params: {
         tags: this.tag,
         page: this.staffs.length / 20 + 1,
       },
     }).then((res) => {
       if (res.data.hits.length) {
         this.staffs = this.staffs.concat(res.data.hits);
         this.$refs.infiniteLoading.$emit('$InfiniteLoading:loaded');
         if (this.staffs.length / 20 === 10) {
           this.$refs.infiniteLoading.$emit('$InfiniteLoading:complete');
         }
       } else {
         this.$refs.infiniteLoading.$emit('$InfiniteLoading:complete');
       }
     });
   },

    changeFilter() {
      this.list = [];
      this.$nextTick(() => {
        this.$refs.infiniteLoading.$emit('$InfiniteLoading:reset');
      });
    },

    onClick: function(staff) {
      store.selectedStaff = staff;

    },
    phoneCall:function(number){
      window.location.href = "tel:"+number

    },

    importStaff:function(){
      this.$http.post('http://mcampus.hsmc.edu.hk/cyrus/test_contact.php')
        .then(response => {

    //  this.loading = true;

          let result = response.data[0].Rows;
          store.staffs = result;
          self.$data.staffs = result;
          addData(self.staffs);
          // this.loading = false;
          // window.f7.hidePreloader();

        }, {
          headers: {
            'X-CSRF-Token': undefined
          }
        })
    },

    sendMail:function(){
      window.location.href = "mailto:sales@withheld.com"
    },
    showLoading:function(){
        window.f7.showPreloader();
    },
    hideLoading:function(){
        window.f7.hidePreloader();
    },
    // onInfinite: function () {
    //         if (this.staffs.length > 200) {
    //           this.$refs.infiniteLoading.$emit('$InfiniteLoading:complete');
    //         } else {
    //           setTimeout(function () {
    //             var temp = [];
    //             for (var i = this.staffs.length; i <= this.staffs.length + 10; i++) {
    //               temp.push(i);
    //             }
    //             this.staffs = this.staffs.concat(temp);
    //             this.$refs.infiniteLoading.$emit('$InfiniteLoading:loaded');
    //           }.bind(this), 1000);
    //         }
    //       },
    //       changeFilter() {
    //   this.staffs = [];
    //   this.$nextTick(() => {
    //     this.$refs.infiniteLoading.$emit('$InfiniteLoading:reset');
    //   });
    // }

  },
  created() {

    self = this;
// showLoading();
    // window.f7.showPreloader();



        this.$http.post('http://mcampus.hsmc.edu.hk/cyrus/test_contact_v2.php')
          .then(response => {
            let result = response.data[0].Rows;
            self.$data.departments = result;


          }, {
            headers: {
              'X-CSRF-Token': undefined
            }
          })

    this.$http.post('http://mcampus.hsmc.edu.hk/cyrus/test_contact.php')
      .then(response => {

  //  this.loading = true;

        let result = response.data[0].Rows;
        store.staffs = result;
        self.$data.staffs = result;
        addData(self.staffs);
        // this.loading = false;
        // window.f7.hidePreloader();

      }, {
        headers: {
          'X-CSRF-Token': undefined
        }
      })

      //
      // hideLoading();
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
  },
  components: {
    InfiniteLoading,
  }


}
</script>
<style>
.hsmcstaff .item-media img {
  border-radius: 100%;
  height: 80px;
  width: 80px;
  background-size: cover;
  /*border:1px solid #021a40;*/
}






</style>
