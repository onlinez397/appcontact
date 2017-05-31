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
					<f7-nav-center sliding>Framework7</f7-nav-center>
					<f7-nav-right>
						<f7-link icon="icon-bars" open-panel="right"></f7-link>

						</f7-link>
					</f7-nav-right>



				</f7-navbar>




				<!-- Pages -->

				<f7-pages>
					<f7-page>

						<f7-searchbar cancel-link="Cancel" placeholder="Search in items" :clear="true" v-model="searchme"></f7-searchbar>

						<f7-list form >
							<!-- Enables Smart Select behavior by adding "smart-select" prop -->
						 <f7-list-item smart-select title="Fruit"  smart-select-open-in="picker">
								<!-- Select with values inside -->
								<select name="fruits" v-model="searchdept">
									  <option value="" selected>Please select department ...</option>
									<option v-for="department in departments" :value="department.fullname">{{department.fullname}}</option>

								</select>
							 </f7-list-item>
							<!-- Multiple Smart Select With Searchbar -->

						</f7-list>


<!--
 <f7-list-item>
     <f7-label>Name</f7-label>
     <f7-input type="text" placeholder="Name" v-model="searchme"/>
   </f7-list-item> -->
	 <!-- <input v-model="searchme" class="form-control"> -->

						<!-- <f7-block-title>Welcome to my App</f7-block-title>
						<f7-block inner>
							<p>Duis sed erat ac eros ultrices pharetra id ut tellus. Praesent rhoncus enim ornare ipsum aliquet ultricies. Pellentesque sodales erat quis elementum sagittis.</p>
						</f7-block>
						<f7-block-title>Navigation</f7-block-title> -->
						   <!-- <tr v-for="source in filterBy(sources, searchme) " >
								 <td>{{source.display_name}}</td> -->
						<f7-list　 class="hsmcstaff" media-list="">


								<!-- <f7-list-item v-for="source in filterBy(sources, searchme) " >{{source.display_name}}</f7-list-item> -->

								<f7-list-item link="#" v-on:click="onClick()" v-for="source in filterBy(sources, searchdept)"  media='<img src ="https://expertbeacon.com/sites/default/files/advice_for_men_on_selecting_your_online_dating_profile_photo.jpg">' :title="source.display_name"   :subtitle="source.department_fullname" :text="source.ext"
     ></f7-list-item>



						</f7-list>

					</f7-page>
				</f7-pages>
			</f7-view>
		</f7-views>



	</div>
</template>

<script>

let self;

function addData(sources){
	for (let source of sources) {

		source.profile_image_url = '<img src="https://ecampus.hsmc.edu.hk/moodle/myhsmc/staff/staffpicture.php?id='+source.id + '">'
		// 'https://ecampus.hsmc.edu.hk/moodle/myhsmc/staff/staffpicture.php?id='+source.id
	}
}
export default {
	data(){
		        return{
		          sources:[],
		          source: '',
							searchme:'',
							departments:[],
							department:''

		        }
		      },



								 created: function(){
									self= this;

									this.$http.post('http://mcampus.hsmc.edu.hk/cyrus/test_contact.php')
										.then(response => {

											let result = response.data[0].Rows;
												self.$data.sources = result;

										},{headers: {'X-CSRF-Token': undefined}})

										this.$http.post('http://mcampus.hsmc.edu.hk/cyrus/test_contact_v2.php')
											.then(response => {

												let result = response.data[0].Rows;
													self.$data.departments = result;

											},{headers: {'X-CSRF-Token': undefined}})


										addData(self.sources);


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
