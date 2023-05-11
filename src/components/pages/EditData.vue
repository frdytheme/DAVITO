<script setup>
import { ref, reactive, onMounted, computed, nextTick } from "vue";

// 표 제목 ref
const tableTitle = ref("");

const textInput = ref(null);
// 셀 선택 onMounted용 ref
const cellTexts = ref([]);
const col = ref([]);
const row = ref([]);
// 드래그 여부 ref
const isDrag = ref(false);
// 테이블 내 마우스 좌표 계산용 ref
const dragBoxWidth = ref(0);
const dragBoxHeight = ref(0);
const dragOffsetX = ref(0);
const dragOffsetY = ref(0);
const dragBoxTop = ref(0);
const dragBoxLeft = ref(0);

onMounted(() => {
  cellTexts.value = document.querySelectorAll(".cell_text");
  col.value = document.querySelectorAll(".col_index");
  row.value = document.querySelectorAll(".row_index");
});

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
  console.log(cell.id)
  nextTick(() => {
    textInput.value[0].focus()
  })
}
function deactiveCell(cell) {
  cell.active = false;
  textInput.value = null;
}

function selectBox(e) {
  col.value.forEach((col) => col.classList.remove("selected"));
  row.value.forEach((row) => row.classList.remove("selected"));
  cellTexts.value.forEach((cell) => cell.classList.remove("selected"));
  if (colItems.includes(e.target.id) && e.target.id) {
    e.target.classList.add("selected");
    cellTexts.value.forEach((col) => {
      col.id[0] === e.target.id ? col.classList.add("selected") : col.classList.remove("selected");
    });
  } else if (rowItems.includes(e.target.id * 1)) {
    e.target.classList.add("selected");
    cellTexts.value.forEach((row) => {
      row.id.slice(1, row.id.length) === e.target.id ? row.classList.add("selected") : row.classList.remove("selected");
    });
  } else if (e.target.classList.contains("cell_text")) {
    e.target.classList.add("selected");
    col.value.forEach((col) => {
      col.id === e.target.id[0] ? col.classList.add("selected") : col.classList.remove("selected");
    });
    row.value.forEach((row) => {
      row.id === e.target.id.slice(1, e.target.id.length)
        ? row.classList.add("selected")
        : row.classList.remove("selected");
    });
  }
}

function dragOn(e) {

  const tbody = e.currentTarget.getBoundingClientRect();
  let x = e.clientX - tbody.left;
  let y = e.clientY - tbody.top;

  dragOffsetX.value = x;
  dragOffsetY.value = y;

  isDrag.value = true;
}
function dragOff(e) {
  isDrag.value = false;
  dragBoxTop.value = 0;
  dragBoxLeft.value = 0;
  dragOffsetX.value = 0;
  dragOffsetY.value = 0;
  dragBoxWidth.value = 0;
  dragBoxHeight.value = 0;
}
function dragCell(e) {

  const tbody = e.currentTarget.getBoundingClientRect();
  let x = e.clientX - tbody.left;
  let y = e.clientY - tbody.top;

  if (isDrag.value) {
    let newWidth = x - dragOffsetX.value;
    let newHeight = y - dragOffsetY.value;

    dragBoxWidth.value = Math.abs(newWidth);
    dragBoxHeight.value = Math.abs(newHeight);

    // left, top 위치 계산
    dragBoxLeft.value = newWidth < 0 ? x : dragOffsetX.value;
    dragBoxTop.value = newHeight < 0 ? y : dragOffsetY.value;
  }
}

// 드래그 박스 style 객체
const dragBoxStyle = computed(() => ({
  position: "absolute",
  top: `${dragBoxTop.value}px`,
  left: `${dragBoxLeft.value}px`,
  width: `${dragBoxWidth.value}px`,
  height: `${dragBoxHeight.value}px`,
  backgroundColor: "var(--color-orange)",
  opacity: 0.3,
}));
</script>

<template>
  <table border="1" @click="selectBox">
    <caption>
      <input type="text" placeholder="제목없음" v-model="tableTitle" />
    </caption>
    <thead>
      <tr>
        <th v-for="col in colItems" :key="col" class="col_index" :id="col">{{ col }}</th>
      </tr>
    </thead>
    <tbody @mousedown="dragOn" @mouseup="dragOff" @mousemove="dragCell">
      <div class="dragBox" :style="dragBoxStyle"></div>
      <tr v-for="row in rowList" :key="row[0]">
        <th class="row_index" :id="row[0].id">{{ row[0].id }}</th>
        <td v-for="num in row.length - 1" class="cell_item" >
          <input
            type="text"
            class="cell_input"
            v-model="row[num].value"
            v-if="row[num].active"
            @blur="deactiveCell(row[num])"
            ref="textInput" />
          <span v-show="!row[num].active" @dblclick="activeCell(row[num])" class="cell_text" :id="row[num].id">{{ row[num].value }}</span>
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
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
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
table tbody {
  position: relative;
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
  width: 100px;
}
table tbody .cell_input,
table tbody .cell_item .cell_text {
  width: 100%;
  height: 100%;
  border: none;
  outline: none;
  position: absolute;
  top: 0;
  left: 0;
}
table tbody .cell_item .cell_text {
  font-size: 13px;
}
.selected {
  background-color: var(--color-orange);
  color: #fff;
}
</style>
