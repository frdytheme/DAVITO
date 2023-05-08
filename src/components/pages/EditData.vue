<script setup>
import { ref } from "vue";

const tableTitle = ref("");

const alphabetArray = [""];
for (let i = 65; i <= 90; i++) {
  const letter = String.fromCharCode(i);
  alphabetArray.push(letter);
}

const colLength = ref(10);
const rowLength = ref(30);
const colItems = alphabetArray.slice(0, colLength.value);
const rowItems = Array.from({ length: rowLength.value }, (_, idx) => idx + 1);
const cells = colItems.slice(1, colItems.length);
console.log(rowItems);
console.log(colItems);
console.log(cells);
</script>

<template>
  <table border="1">
    <caption>
      <input type="text" placeholder="제목없음" v-model="tableTitle" />
    </caption>
    <thead>
      <tr>
        <th v-for="col in colItems" :key="col" class="col-index">{{ col }}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="row in rowItems" :key="row">
        <th class="row-index">{{ row }}</th>
        <td v-for="cell in cells" :key="cell" class="cell"></td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped>
table {
  width: 95%;
  border: 1px solid #d6d6d6;
  text-align: left;
  margin: 10px;
  /* table-layout: fixed; */
}
table caption {
  margin: 10px 30px 10px 10px;
  text-align: left;
}
table caption input {
  border: none;
  border-bottom: 1px solid #000;
  outline: none;
  width: auto;
  height: 50px;
  font-size: 25px;
}
table thead {
  text-align: center;
  font-weight: normal;
}
table thead .col-index {
  width: auto;
  height: auto;
}
table tbody {
}
table tbody .row-index {
  text-align: center;
  width: 5%;
  height: auto;
  font-size: 12px;
  font-weight: normal;
}
table tbody .cell {
  width: 10%;
}
</style>
