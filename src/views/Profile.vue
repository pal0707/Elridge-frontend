 <template>
  <div class="home">
    <table>
      <tr>
        <th>Id</th>
        <th>Post Created</th>
        <th>Post Title</th>
        <th>Post Description</th>
        <th>Action</th>
      </tr>
      <tr v-for="x in postList">
        <td>{{ x.id }}</td>
        <td>{{ x.created_at }}</td>
        <td>{{ x.title }}</td>
        <td>{{ x.body }}</td>
        <td>
          <button>
            <router-link :to="'/post/' + x.id">View</router-link>
          </button>
        </td>
      </tr>
    </table>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "PostDetail",
  data() {
    return {
      postList: [],
      token: localStorage.getItem("userToken"),
    };
  },
  created() {
    this.getPostList();
  },
  methods: {
    getPostList() {
      axios
        .get(`http://127.0.0.1:8000/post/`, {
          headers: {
            Authorization: "Token " + this.token,
          },
        })
        .then((res) => {
          this.postList = res.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    goTo() {
      this.$router.back();
    },
  },
};
</script>
<style lang="css">
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>