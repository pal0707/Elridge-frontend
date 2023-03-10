 <template>
  <div class="home">
    <ul>
      <li>Post Title : {{ postData.title }}</li>
      <li>Post Description : {{ postData.body }}</li>
      <li>Post Created : {{ postData.created_at }}</li>
    </ul>
  </div>
</template>
<script>
import axios from "axios";

export default {
  name: "PostDetail",
  props: ["id"],
  data() {
    return {
      postData: [],
      token: localStorage.getItem("userToken"),
    };
  },
  created() {
    this.getPostDetail();
  },
  methods: {
    getPostDetail() {
      axios
        .get(`http://127.0.0.1:8000/post/${this.id}/`, {
          headers: {
            Authorization: "Token " + this.token,
          },
        })
        .then((res) => {
          this.postData = res.data;
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