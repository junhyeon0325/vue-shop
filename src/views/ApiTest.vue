<template>
  <div>{{ msg }}</div>
  <form>
    <input type="file" @change="getFileInfo" />
  </form>
  <table class="list">
    <thead>
      <tr>
        <th>상품번호</th>
        <th>상품명</th>
        <th>가격</th>
        <th>Category</th>
      </tr>
    </thead>
    <tbody>
      <tr :key="i" v-for="(value, i) in prodList">
        <td>{{ value.id }}</td>
        <td>{{ value.product_name }}</td>
        <td>{{ value.product_price }}</td>
        <td>
          {{ value.category1 }}/{{ value.category2 }}/{{ value.category3 }}
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import axios from "axios";
// import { asyncDispose } from "core-js/fn/symbol";
export default {
  data() {
    return {
      msg: "API Test",
      prodList: [],
    };
  },
  methods: {
    getFileInfo(e) {
      console.log(e);
      const file_name = e.target.files[0].name;
      console.log(file_name);
      const file = e.target.files[0]; // Stream -> text 변환
      console.log(file);
      let fr = new FileReader(); // filereader는 읽기전용으로 열겠다 라는 의미
      fr.onload = (ev) => {
        let base64Txt = ev.target.result;
        console.log(base64Txt.slice(base64Txt.indexOf(";base64") + 8));
        // base64txt 라는걸 indexof가 그단어를 찾아줌 거기서 8번째까지 짤라가지고(slice)그거를 log에 출력
        const data = base64Txt.slice(base64Txt.indexOf(";base64") + 8);
        axios //
          .post("/upload/" + file_name, { param: data })
          .then((result) => console.log(result));
      }; // 이벤트핸들러 on무슨속성은 이벤트와 관련있음
      fr.readAsDataURL(file);
    },
  },
  mounted() {
    axios({ method: "post", url: "/api/productList", data: {} }) //
      .then((result) => {
        console.log(result);
        this.prodList = result.data;
      })
      .catch((err) => console.error(err));
  },
};
</script>

<style scoped>
table.add {
  width: 70%;
  margin: auto;
  border-collapse: collapse;
}

.add th,
.add td {
  border: 2px solid pink;
}

table.list {
  width: 100%;
  margin: auto;
  border-collapse: collapse;
}

.list th,
.list td {
  border: 2px solid green;
}
</style>
