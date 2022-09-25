<template>
	<section>
		<h1>Assigned Staff</h1>
		<div v-if="!loaded">
			<div class="spinner-border text-primary" role="status"></div>
		</div>

		<div  v-else>
			<button type="button" class="btn btn-primary mb-3" @click="loadAssignedStaff">Refresh data</button>
			<table id="tableComponent" class="table table-bordered table-striped">
				<thead>
					<tr>
						<th>First Name</th>
						<th>Last Name</th>
						<th>Maiden Name</th>
						<th>Age</th>
						<th>Gender</th>
						<th>Email</th>
						<th>Phone</th>
						<th>Username</th>
						<th>Password</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="item in assignedStaff" :key='item'>
						<td>{{item.firstName}}</td>
						<td>{{item.lastName}}</td>
						<td>{{item.maidenName}}</td>
						<td>{{item.age}}</td>
						<td>{{item.gender}}</td>
						<td>{{item.email}}</td>
						<td>{{item.phone}}</td>
						<td>{{item.username}}</td>
						<td>{{item.password}}</td>
					</tr>
				</tbody>
			</table> 
		</div>
	</section>
</template>

<script>
export default {
	name: "AssignedStaff",
	data() {
		return {
			// this is an empty array by default. It holds the response from the api
			assignedStaff: [],
			error: null,
			url: "",
			loaded: false
		}
	},
	methods: {
		// This method makes an API call and set the response to a variable in the component data()
		
		loadAssignedStaff() {
			this.loaded = false;
			fetch(`${this.url}`)
				.then(response => {
					return response.json()
				})
				.then(data => {
					this.assignedStaff = data.users;
					this.loaded = true;
				})
				.catch(error => {
					this.error = error;
				});
		}
	},
	mounted() {
		// this makes the app agnostic to the api endpoint plugged to the app;
		this.url = process.env.VUE_APP_URL;
		// call loadAssignedStaff methods so that the responds from the methods is mounted on the DOM by the time the page finish loading
		this.loadAssignedStaff();
	}
};
</script>
