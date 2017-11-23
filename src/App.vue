<template>
  <form @submit.prevent="sendForm" id="app">
    <el-row>
      <el-row>
        <el-col :xs="24">
          <h1>Габаритные и количественные данные объектов</h1>
        </el-col>
    </el-row>
    <div class="margined-bottom"v-for="item in items" :key="item.id">
      <!-- Change span to :xs="8" :sm="6" :md="4" :lg="3" :xl="1" -->
      <el-row :gutter="20">
        <el-col :lg="1">
          <el-button disabled>
            {{item.id}}
          </el-button>
        </el-col>
        <el-col :xs="24" :lg="11" :xl="5">
          <el-input type="text" v-model="item.name"/>
        </el-col>
        <el-col :xs="24" :lg="3">
          <el-input-number controls-position="right" v-model="item.width"/>
        </el-col>
        <el-col :xs="24" :lg="3">
          <el-input-number controls-position="right" v-model="item.height"/>
        </el-col>
        <el-col :xs="24" :lg="3">
          <el-input-number controls-position="right" v-model="item.length"/>
        </el-col>
        <el-col :offset="2" :xs="24" :lg="1">
          <el-button type="danger" @click="deleteItem(item.id)">
            <i class="el-icon-minus"></i>
          </el-button>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :xs="24" :lg="3">
          <el-input type="text"/>
        </el-col>
        <el-col :xs="24" :lg="3">
          <el-input type="text"/>
        </el-col>
        <el-col :xs="24" :lg="3">
          <el-input type="text"/>
        </el-col>
        <el-col :xs="24" :lg="2">
          <el-radio-group v-model="item.radio">
            <el-radio-button label="1"></el-radio-button>
            <el-radio-button label="2"></el-radio-button>
          </el-radio-group>
        </el-col>
        <el-col :xs="24" :lg="2" :offset="2">
          <el-radio-group v-model="item.radio2">
            <el-radio-button label="A"></el-radio-button>
            <el-radio-button label="B"></el-radio-button>
          </el-radio-group>
        </el-col>
        <el-col :xs="24" :lg="2" :offset="2">
          <el-input placeholder="П-1-1-ш" v-model="input1" :disabled="true">
          </el-input>
        </el-col>
        <el-col :xs="24" :lg="4" :offset="1" >
          <el-input-number class="last-input" v-model="params.calc0" :min="1" :max="10" controls-position="right"/>
        </el-col>
      </el-row>
    </div>
    </el-row>
    <el-row>
      <el-row>
        <el-col :xs="24" :lg="21">
          <h1>Данные для расчета услуг</h1>
        </el-col>
        <el-col :xs="24" :lg="3">
          <el-button class="add" @click="addItem">
            <i class="el-icon-plus"></i>
            Добавить строку
          </el-button>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :xs="24" :lg="4">
          <el-input-number v-model="params.calc1" :min="1" :max="10" controls-position="right"/>
        </el-col>
        <el-col :xs="24" :lg="4">
          <el-input-number v-model="params.calc2" :min="0" :max="10" controls-position="right"/>
        </el-col>
        <el-col :xs="24" :lg="4">
          <el-input-number v-model="params.calc3" :min="1000" :max="3000" controls-position="right"/>
        </el-col>
        <el-col :xs="24" :lg="4">
          <el-input-number v-model="params.calc4" :min="1" :max="10" controls-position="right"/>
        </el-col>
        <el-col :xs="24" :lg="4">
        <el-dropdown split-button type="primary">
          Зона
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item>Зона 1</el-dropdown-item>
            <el-dropdown-item>Зона 2</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
        </el-col>
        <el-col :xs="24" :lg="4">
          <el-input-number v-model="num2" :disabled="true"></el-input-number>
        </el-col>
        </el-row>
        <el-row>
          <el-col  :xs="24" :lg="4" :offset="20">
            <el-button class="save" @click="sendForm" type="submit">Coхранить и рассчитать</el-button>
          </el-col>
        </el-row>
      </el-row>
  </form>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      input1: '',
      items: [],
      params: {
        calc0: 0,
        calc1: 1,
        calc2: 0,
        calc3: 2000,
        calc4: 10
      },
      i: 0,
      num2: 1
    }
  },
  methods: {
    addItem () {
      this.items.push({
        id: ++this.i,
        radio: '1',
        radio2: 'A',
        name: '',
        width: 10,
        height: 0,
        length: 0
      })
    },
    deleteItem (id) {
      this.items = this.items.filter((i) => i.id !== id)
    },
    sendForm () {
      const data = JSON.stringify({
        items: this.items,
        params: this.params
      })
      fetch('//place-your-url.here', {method: 'POST', body: data})
    }
  }
}
</script>

<style lang="sass">
@import url('https://fonts.googleapis.com/css?family=Roboto')
*
 font-family: 'Roboto', sans-serif

h1
  font-weight: 100
.el-row
  padding: 10px
.el-col
  & > .last-input, & > .el-button
    width: 100% !important
  & > .el-button
    padding: 12px
.add
  color: blue
  border-color: blue 
  width: 100%
.el-button--danger
  background-color: white
  border: 1px solid red 
  width: 100%
  .el-icon-minus
    color: red
    font-weight: bold
  
.el-radio-button__orig-radio:checked+.el-radio-button__inner
  background-color: #B3B5B5
  border-color: gray
  box-shadow: -1px 0 0 0 grey
.el-button--primary
  background-color: white
  color: black
  
.el-input-number
  width: 100%
.save
  background-color: #19B4A2
  color: white
.margined-bottom
  margin-bottom: 30px

</style>
