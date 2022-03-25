<template>
  <div>
    <!-- <div id="btnContainer">
      <button class="btnon" v-on:click="listdata" style="border-radius: 50%">
        <i class="fa fa-bars"></i>
      </button>
      &nbsp;&nbsp;
      <button
        class="btnon active"
        v-on:click="griddata"
        style="border-radius: 50%"
      >
        <i class="fa fa-th-large"></i>
      </button>
    </div> -->
  </div>
  <br /><br />
  <ul class="nav">
    <li id="length">
      <button class="btn btn-default">
        <b> {{ datalist.length }}</b
        ><sub>result</sub>
      </button>
    </li>
    <li>
      <input
        type="text"
        v-model="search"
        placeholder="Search companyname"
        id="search"
      />&nbsp; &nbsp;
    </li>
    <li>
      <div id="btnContainer">
        <button class="btnon" v-on:click="listdata" style="border-radius: 50%">
          <i class="fa fa-bars"></i>
        </button>
        &nbsp;&nbsp;
        <button
          class="btnon active"
          v-on:click="griddata"
          style="border-radius: 50%"
        >
          <i class="fa fa-th-large"></i>
        </button>
      </div>
    </li>
    <li>
      <label class="drop">
        <select @click="pagenationdata($event)" class="select">
          <!-- <option value="5">5</option>
          <option value="7">7</option>
          <option value="10">10</option> -->
          <option v-for="perpage of perpagerecords" :key="perpage">
            {{ perpage }}
          </option>
        </select>
      </label>
    </li>
    <li id="perpage">
      <span style="font-size: 12px">perpage</span>&nbsp;&nbsp;
    </li>
    <li id="perpage1">
      <i class="fas fa-align-justify" style="font-size: 24px"></i>
    </li>
  </ul>

  <div class="container">
    <table v-show="gridvisible">
      <tr>
        <th>
          <label class="form-checkbox">
            <input
              type="checkbox"
              v-model="selectAll"
              @click="select"
            />&nbsp;&nbsp;
            <i class="fas fa-check" style="font-size: 12px"></i>
            <i class="form-icon"></i>
          </label>
        </th>
        <th>Id</th>
        <th>CompanyName</th>
        <th>Email</th>
        <th>State</th>
      </tr>
      <tr v-for="(user, i) of filterdata" :key="i">
        <td>
          <label class="form-checkbox">
            <input type="checkbox" :value="i" v-model="selected" id="chek" />
            <i class="form-icon"></i>
          </label>
        </td>
        <td>{{ i + 1 }}</td>
        <td>{{ user.CompanyName }}</td>
        <td style="color: blueviolet">{{ user.email }}</td>
        <td>{{ user.state }}</td>
      </tr>
    </table>
  </div>

  <div
    class="container row"
    v-show="listvisible"
    v-for="(user, i) of filterdata"
    :key="i"
  >
    <hr />
    <div class="col-md-6">
      <label class="form-checkbox" >
        <input
          class="check"
          type="checkbox"
          :value="i"
          v-model="selected"
          @click="select"
        />
        <!-- <i class="form-icon" ></i>  -->
      </label>
      id<br>
      <b>{{ i + 1 }}</b
      ><br />CompanyName<br />{{ user.CompanyName }}<br />
    </div>
    <div class="col-md-6">
      Email<br />
      <p style="color: blueviolet">{{ user.email }}</p>
      State <br />{{ user.state }}
    </div>
    &nbsp;
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "GetData",
  data() {
    return {
      userdetails: [],
      search: "",
      selectedAll: false,
      selected: [],
      datalist: [],
      pagination: 0,
      listvisible: false,
      gridvisible: true,
      perpagerecords: [2,5,10,20],
      seletedVal: 0,
      filteredData: [],
    };
  },
  computed: {
    filterdata() {
      return this.datalist.filter((post) => {
        return post.CompanyName.toLowerCase().includes(
          this.search.toLowerCase()
        );
      });
    },
  },

  mounted() {
    this.adddata();
  },
  methods: {
    adddata() {
      console.log(this.userdetails);

      axios

        .get("https://vuefirebasetask-default-rtdb.firebaseio.com/post.json")

        .then((response) => {
          this.userdetails = response.data;

          console.log(this.userdetails);

          Object.keys(this.userdetails).forEach((result) => {
            console.log(result);

            this.datalist.push(this.userdetails[result]);
            this.filteredData = this.datalist;
          });
        });
    },
    pagenationdata(e) {
      this.seletedVal = e.target.value;

      if (this.seletedVal !== 0) {
        console.log(this.seletedVal);

        this.filteredData = this.filterdata.slice(0, this.seletedVal);
      }
    },

    listdata() {
      this.listvisible = false;
      this.gridvisible = true;
    },
    griddata() {
      this.gridvisible = false;
      this.listvisible = true;
    },
  },
  select() {
    this.selected = [];
    console.log("hi");
    if (!this.selectedAll) {
      for (let i in this.userdetails) {
        this.selected.push(this.userdetails[i].id);
      }
    }
  },
};
</script>
<style>
table {
  margin-top: 35px;
}
table {
  border-collapse: collapse;
  width: 100%;
}

th,
td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.inline {
  display: inline;
  width: 70%;
  height: 60%;
}
.total {
  margin-right: 1000px;
}
.res {
  float: left;
}

.tab {
  width: 70%;
  margin-left: 30%;
  margin-right: 30%;
}
#id {
  margin-right: 20px;
}
.one {
  margin-left: 10px;
}
.row {
  display: flex;
  flex-wrap: wrap;
  margin-right: 400px;
  margin-left: 80px;
  border: 1px solid black;
  border-radius: 10px;
  padding-bottom: 5px;
  width: 85%;
}
.btnon {
  border: none;
  outline: none;
  padding: 12px 16px;
  background-color: #f1f1f1;
  cursor: pointer;
  margin-top: -7px;
}

.style {
  display: inline;
  margin-left: 100px;
  margin-top: 50px;
}
.search {
  margin-left: 1050px;
  width: 300px;
}
.drop {
  margin-left: 250px;
  height: 50px;
  width: 50px;
  margin-bottom: 15px;
}
#perpage1 {
  margin-right: 5px;
  width: 50px;
}
#perpage {
  margin-top: -7px;
  margin-right: 5px;
  height: 70px;
}

.btnon:hover {
  background-color: #ddd;
}

.btnon.active {
  background-color: #666;
  color: white;
}

.grid-row {
  display: table-row;
}

.app-gridview {
  display: grid;
}

.page {
  margin-top: -30px;
}

.nav {
  height: 60px;
  width: 1140px;
  display: inline-block;
}

.nav li {
  float: left;
  list-style-type: none;
  position: relative;
  display: inline;
}

.select {
  margin-bottom: 200px;
}
#search {
  width: 357px;
  margin: 4px;
  margin-left: 100px;
}
.check{
      margin-left: -260px;
    margin-bottom: auto;
   
}
</style>