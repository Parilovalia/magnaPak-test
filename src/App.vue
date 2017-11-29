<template>
  <form @submit.prevent="sendForm" id="app">
    <el-row>
      <el-row>
        <el-col :xs="24">
          <h1>Габаритные и количественные данные объектов</h1>
        </el-col>
    </el-row>
    <div class="margined-bottom"v-for="(item, index) in items" :key="item.index">
      <!-- Change span to :xs="8" :sm="6" :md="4" :lg="3" :xl="1" -->
      <el-row :gutter="20">
        <el-col :lg="1">
          <el-button disabled>
            {{index+1}}
          </el-button>
        </el-col>
        <el-col :xs="24" :lg="11" :xl="5">
          <el-input type="text" v-model="item.name"/>
        </el-col>
        <el-col :xs="24" :lg="3">
          <el-input-number class="fixed" controls-position="right" v-model="item.x"/>
        </el-col>
        <el-col :xs="24" :lg="3">
          <el-input-number controls-position="right" v-model="item.y"/>
        </el-col>
        <el-col :xs="24" :lg="3">
          <el-input-number controls-position="right" v-model="item.z"/>
        </el-col>
        <el-col :offset="2" :xs="24" :lg="1">
          <el-button type="danger" @click="deleteItem(item.id)">
            <i class="el-icon-minus"></i>
          </el-button>
        </el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :xs="24" :lg="3">
          <el-input type="text" v-model.number="item.width" @blur="handleEq(item)"/>
        </el-col>
        <el-col :xs="24" :lg="3">
          <el-input type="text" v-model.number="item.height" @blur="handleEq(item)" :disabled="!item.volume"/>
        </el-col>
        <el-col :xs="24" :lg="3">
          <el-input type="text" v-model.number="item.lenght" @blur="handleEq(item)"/>
        </el-col>
        <el-col :xs="24" :lg="2">
          <el-radio-group v-model="item.volume">
            <el-radio-button :value="true">
              <img src="src/assets/polygon.png"/>
            </el-radio-button>
            <el-radio-button :value="false" label="2">
              <img src="src/assets/box.png"/>
            </el-radio-button>
          </el-radio-group>
        </el-col>
        <el-col :xs="24" :lg="2" :offset="2">
          <el-radio-group v-model="item.isBSelected">
            <el-radio-button :value="true" :disabled="!item.volume" label="A"></el-radio-button>
            <el-radio-button :value="false" label="B" :disabled="false"></el-radio-button>
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
          <el-input-number v-model="params.calc1" :min="1" :max="10" controls-position="right" class="fixed2"/>
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
        <el-select placeholder="Зона" v-model="zone" split-button type="primary">
          <el-option value="Зона 1">Зона 1</el-option>
          <el-option value="Зона 2">Зона 2</el-option>
          <el-option value="ФР">ФР</el-option>
        </el-select>
        </el-col>
        <el-col :xs="24" :lg="4">
          <el-input-number v-model="num2" :disabled="zone!=='ФР'"></el-input-number>
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
      num2: 1,
      zone: ''
    }
  },
  methods: {
    handleEq (inputEl) {
      // const a = parseInt(value)
      // value = a.toFixed(1)
      const item = this.items.find(function(el) {
        return el == inputEl
      })
      item.width = item.width.toFixed(1)
      item.height = item.height.toFixed(1)
      item.lenght = item.lenght.toFixed(1)
    },
    addItem () {
      this.items.push({
        id: ++this.i,
        radio: '1',
        radio2: 'A',
        name: '',
        x: 10,
        y: 0,
        z: 0,
        width: 0,
        height: 0,
        lenght: 0,
        volume: true,
        isBSelected: true
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
