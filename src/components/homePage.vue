<script setup>
import { ref } from 'vue'

// const products = ref([])

const products = ref([
  { id: 0, title: '珍珠奶茶', detail: '香濃奶茶搭配QQ珍珠', price: 50, count: 20, edit: false },
  { id: 1, title: '冬瓜檸檬', detail: '清新冬瓜配上新鮮檸檬', price: 45, count: 18, edit: false },
  { id: 2, title: '翡翠檸檬', detail: '綠茶與檸檬的完美結合', price: 55, count: 34, edit: false },
  { id: 3, title: '四季春茶', detail: '香醇四季春茶，回甘無比', price: 45, count: 10, edit: false },
  {
    id: 4,
    title: '阿薩姆奶茶',
    detail: '阿薩姆紅茶搭配香醇鮮奶',
    price: 50,
    count: 25,
    edit: false
  },
  { id: 5, title: '檸檬冰茶', detail: '檸檬與冰茶的清新組合', price: 45, count: 20, edit: false },
  { id: 6, title: '芒果綠茶', detail: '芒果與綠茶的獨特風味', price: 55, count: 18, edit: false },
  { id: 7, title: '抹茶拿鐵', detail: '抹茶與鮮奶的絕配', price: 60, count: 20, edit: false }
])
const editContent = ref([])

const selIdx = (id) => products.value.findIndex((item) => item.id === id)
const editCancel = (editDate) => {
  const idx = selIdx(editDate ? editDate.id : editContent.value[0].id)
  products.value[idx].edit = false
  editContent.value.length = 0
}
const updateCount = (id, num) => {
  const idx = selIdx(id)
  if (products.value[idx].count || num > 0) products.value[idx].count += num
}

const editProduct = (id) => {
  const idx = selIdx(id)
  if (editContent.value.length) editCancel({ ...editContent.value[0] })
  editContent.value.push({ ...products.value[idx] })
  products.value[idx].edit = true
}

const saveProduct = () => {
  const idx = selIdx(editContent.value[0].id)
  products.value[idx] = { ...editContent.value[0] }
  editCancel()
}

// document.onreadystatechange = function () {
//   if (document.readyState == 'complete') {
//     const getTable = document.querySelectorAll('#originalTable tr')
//     getTable.forEach((element, key) => {
//       const item = element.querySelectorAll('td')
//       products.value.push({
//         id: key,
//         title: item[0].textContent,
//         detail: item[1].textContent,
//         price: parseInt(item[2].textContent),
//         count: parseInt(item[3].textContent) * -1
//       })
//       // text.value += `{
//       // id:${key},
//       // title: '${item[0].textContent}',
//       // detail: '${item[1].textContent}',
//       // price: ${item[2].textContent},
//       // count: ${parseInt(item[3].textContent) * -1}},`
//     })
//     // console.log(products.value)
//   }
// }
</script>

<template>
  <header class="py-5 text-center"><h1>Vue homework - week1</h1></header>
  <div class="container-md" style="max-width: 900px">
    <table class="table text-center table-borderless align-middle">
      <thead>
        <tr>
          <th scope="col" class="border">品項</th>
          <th scope="col" class="border">描述</th>
          <th scope="col" class="border">價格</th>
          <th scope="col" class="border" colspan="3" style="max-width: 2em">庫存</th>
          <th scope="col" class="border"></th>
        </tr>
      </thead>
      <tbody v-for="product in products" :key="product.id">
        <tr :class="product.edit ? 'editHid' : ''">
          <td>{{ product.title }}</td>
          <td>
            <small>{{ product.detail }}</small>
          </td>
          <td>{{ product.price }}</td>
          <td>
            <button
              type="button"
              class="btn btn-light"
              @click="updateCount(product.id, -1)"
              v-show="!product.edit"
            >
              -
            </button>
          </td>
          <td style="max-width: 2rem" :style="{ color: product.count ? '' : 'red' }">
            {{ product.count ? product.count : '缺貨' }}
          </td>
          <td>
            <button
              type="button"
              class="btn btn-light"
              @click="updateCount(product.id, 1)"
              v-show="!product.edit"
            >
              +
            </button>
          </td>
          <td>
            <button
              type="button"
              class="btn btn-primary"
              @click="editProduct(product.id)"
              v-show="!product.edit"
            >
              編輯
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <div v-if="editContent.length">
    <hr />
    <h4 class="text-center">品項內容調整</h4>
    <div class="container">
      <div class="row">
        <div class="col-md-3">
          <label for="editTitle" class="form-label">品項</label>
          <input id="editTitle" class="form-control" v-model="editContent[0].title" />
        </div>
        <div class="col-md-5">
          <label for="editDetail" class="form-label">描述</label>
          <input id="editDetail" class="form-control" v-model="editContent[0].detail" />
        </div>
        <div class="col-md-2">
          <label for="editPrice" class="form-label">價格</label>
          <input id="editPrice" class="form-control" v-model.number="editContent[0].price" />
        </div>
        <div class="col-md-2">
          <label for="editCount" class="form-label">庫存</label>
          <input id="editCount" class="form-control" v-model.number="editContent[0].count" />
        </div>
        <div class="col-md-12 text-center">
          <button type="button" class="mx-2 my-3 btn btn-primary btn-sm" @click="saveProduct()">
            確認
          </button>
          <button
            type="button"
            class="mx-2 my-3 btn btn-outline-secondary btn-sm"
            @click="editCancel()"
          >
            取消
          </button>
        </div>
      </div>
    </div>
  </div>
  <table id="originalTable" style="display: none">
    <tr>
      <td>珍珠奶茶</td>
      <td><small>香濃奶茶搭配QQ珍珠</small></td>
      <td>50</td>
      <td><button>-</button>20<button>+</button></td>
    </tr>
    <tr>
      <td>冬瓜檸檬</td>
      <td><small>清新冬瓜配上新鮮檸檬</small></td>
      <td>45</td>
      <td><button>-</button>18<button>+</button></td>
    </tr>
    <tr>
      <td>翡翠檸檬</td>
      <td><small>綠茶與檸檬的完美結合</small></td>
      <td>55</td>
      <td><button>-</button>34<button>+</button></td>
    </tr>
    <tr>
      <td>四季春茶</td>
      <td><small>香醇四季春茶，回甘無比</small></td>
      <td>45</td>
      <td><button>-</button>10<button>+</button></td>
    </tr>
    <tr>
      <td>阿薩姆奶茶</td>
      <td><small>阿薩姆紅茶搭配香醇鮮奶</small></td>
      <td>50</td>
      <td><button>-</button>25<button>+</button></td>
    </tr>
    <tr>
      <td>檸檬冰茶</td>
      <td><small>檸檬與冰茶的清新組合</small></td>
      <td>45</td>
      <td><button>-</button>20<button>+</button></td>
    </tr>
    <tr>
      <td>芒果綠茶</td>
      <td><small>芒果與綠茶的獨特風味</small></td>
      <td>55</td>
      <td><button>-</button>18<button>+</button></td>
    </tr>
    <tr>
      <td>抹茶拿鐵</td>
      <td><small>抹茶與鮮奶的絕配</small></td>
      <td>60</td>
      <td><button>-</button>20<button>+</button></td>
    </tr>
  </table>
</template>

<style>
.editHid {
  background-color: #eee;
  color: #bbb;
}
</style>
