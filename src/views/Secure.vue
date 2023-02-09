<template>
  <div class="secure">
    <h1>Welcome {{ $parent.user.firstName }} {{ $parent.user.lastName }}</h1>
    <h3>{{ $parent.user.email }}</h3>
    <button type="button" v-on:click="getProjects()">Get/Refresh Projects</button>
    <div v-if="projects.length">
      <table class="center">
        <caption>TMS Projects <span class="badge">({{ count }})</span></caption>
        <thead>
          <tr>
            <th><!-- Intentionally Blank --></th>
            <th>Status</th>
            <th>Source Lang</th>
            <th>Target Langs</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in projects">
            <th>{{item.name}}</th>
            <td>{{item.status}}</td>
            <td>{{item.sourceLang}}</td>
            <td>{{item.targetLangs}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>


<script>
    import $ from "jquery";

    export default {
        name: 'Secure',
        data() {
          return {
            projects: [],
            count: 0
          }
        },
        mounted() {
            if(!this.$parent.authenticated) {
                this.$router.replace({ name: "Login" });
            }
        },
        methods: {
            getProjects() {
                let _this = this;
                $.ajax({
                    url: 'http://localhost:8081/api/v1/projects/' + _this.$parent.token,
                    type: 'GET',
                    crossDomain: true,
                    dataType: 'json',
                    contentType: 'application/json',
                    success: function(data) {
                        console.log(data);
                        _this.projects = data.projects;
                        _this.count = data.numberOfProjects
                    },
                    error: function(xhr){
                        console.log(xhr);
                        _this.$parent.logout();
                        _this.$router.replace({ name: 'Login' });
                    }
                });
            }
        }
    }
</script>

<style>
.center {
  margin-left: auto;
  margin-right: auto;
}

.badge {
  font-size: 60%;
}

table {
	border-collapse: collapse;
	border: 1px solid black;
	text-align: center;
	vertical-align: middle;
}

caption {
	font-weight: bold;
	font-size: 24px;
	text-align: left;
	color: #333;
	margin-bottom: 16px;
}

thead {
	background-color: #333;
	color: white;
	font-size: 0.875rem;
	text-transform: uppercase;
	letter-spacing: 2%;
}

th, td {
    border: 1px solid black;
    padding: 8px;
}

tbody tr:nth-child(odd) {
    background-color: #fff;
}
  
tbody tr:nth-child(even) {
    background-color: #eee;
}

tbody th {
    background-color: #36c;
    color: #fff;
    text-align: left;
}

tbody tr:nth-child(even) th {
    background-color: #25c;
}
</style>