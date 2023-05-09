<script setup>
import { ref, reactive, onMounted } from "vue";

const tableTitle = ref("");

// [A-Z] 배열 생성
function collArray() {
  const alphabetArray = [""];
  for (let i = 65; i <= 90; i++) {
    const letter = String.fromCharCode(i);
    alphabetArray.push(letter);
  }
  return alphabetArray.slice(0, 10);
}

const colItems = collArray();
const rowItems = Array.from({ length: 30 }, (_, idx) => idx + 1);
const rowList = reactive(
  rowItems.map((row) => {
    return colItems.map((col) => {
      return { id: col + row, value: "", active: false, col: col, row: row };
    });
  })
);

function activeCell(cell) {
  cell.active = true;
}
function deactiveCell(cell) {
  cell.active = false;
}
</script>

<template>
  <table border="1">
    <caption>
      <input type="text" placeholder="제목없음" v-model="tableTitle" />
    </caption>
    <thead>
      <tr>
        <th v-for="col in colItems" :key="col" class="col_index" :id="col">{{ col }}</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="row in rowList" :key="row[0]">
        <th class="row_index" :id="row[0]">{{ row[0].id }}</th>
        <td v-for="num in row.length - 1" class="cell_item" @dblclick="activeCell(row[num])" :id="row[num].id">
          <input
            type="text"
            class="cell_input"
            v-model="row[num].value"
            v-if="row[num].active"
            @blur="deactiveCell(row[num])"
            autofocus />
          <span v-else @dblclick="activeCell(row[num])">{{ row[num].value }}</span>
        </td>
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
table thead .col_index {
  width: auto;
  height: auto;
}
table tbody .row_index {
  text-align: center;
  width: 5%;
  height: auto;
  font-size: 12px;
  font-weight: normal;
}
table tbody .cell_item {
  position: relative;
}
table tbody .cell_input {
  width: 100%;
  height: 100%;
  border: none;
  outline: none;
  position: absolute;
  top: 0;
  left: 0;
}
</style>
