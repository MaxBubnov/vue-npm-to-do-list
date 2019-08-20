<template>
  <div id="app">
    <div class="add-controls">
      <input type="text" class="add-input" @keydown.enter="deadlineBlockDisplay = true" placeholder="Write here what you need to do!" v-model="currentItem">
      <div class="add-button" @click="deadlineBlockDisplay = true">Add</div>
      <div class="filter-button" @click="filterImp = !filterImp">!Filter</div>
    </div>
    <div class="search-panel">
      <input type="text" class="search-input" placeholder="Have you lost your task? Search for it!" v-model="searchText">
      <div class="cansel-search-button" @click="searchText = ''"><i class="fas fa-times"></i></div>
    </div>
    <div class="list-item-container">
        <ListItem v-for="item in list"
                  :inputText="item.text"
                  :identyfyNumber="item.identify"
                  :importantProp="item.importantTitle"
                  :doneProp="item.done"
                  :deadlineProp="item.deadline"
                  @deleteItem="deleteList(item.identify)"
                  @importantChange="importantStateInListChange(item.identify)"
                  @doneChange="doneStateInListChange(item.identify)"
                  @itemTextChanging="textStateInListChange"
                  :key="item.identify"
                  v-if="(filterImp || item.importantTitle) && (searchText === '' || item.text.includes(searchText))"/>
    </div>
    <DateSettings @cancelDeadline="addItem"
                  @deadlineInstruction="addItem"
                  v-if="deadlineBlockDisplay"/>
  </div>
</template>

<script>
import ListItem from './components/ListItem.vue';
import DateSettings from './components/DateSettings';
const cache = JSON.parse(localStorage.getItem("list")) || [];
console.log(cache);
export default {
  name: 'app',
  components: {
    ListItem,
    DateSettings
  },
  data: function(){
      return {
        currentItem: '',
        list: cache,
        searchText: '',
        filterImp: true,
        deadlineBlockDisplay: false,
        currentItemDeadline: false,
        deadlineCancel: false
      }
  },
  methods: {
    addItem(deadlineData) {
      console.log(deadlineData.timer)
      this.deadlineBlockDisplay = false;
      this.list.unshift({text: this.currentItem, importantTitle: false, done: false, deadline: deadlineData.timer, identify: Date.now()});
      this.currentItem = '';
      this.saveList();
      console.log(this.list);
    },
    deleteList(ident) {
      this.list.forEach((item, i) => {
        if(item.identify === ident){
          this.list.splice(i, 1);
        }
      });
      this.saveList();
    },
    importantStateInListChange(ident){
      this.list.forEach(item => {
        if(item.identify === ident){
          item.importantTitle = !item.importantTitle;
        }
      });
      this.saveList();
    },
    doneStateInListChange(ident){
      this.list.forEach(item => {
        if(item.identify === ident){
          item.done = !item.done;
        }
      });
      this.saveList();
    },
    textStateInListChange(data){
      console.log(data);
      this.list.forEach(item => {
        if(item.identify === data.identForTextChanging){
          item.text = data.changedText;
          console.log("It's WORKING!!!", this.list);
        }
      });
      this.saveList();
    },
    saveList: function (){
      localStorage.setItem("list", JSON.stringify(this.list));
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 90%;
  margin: 30px auto;
}
input{
  outline: none;
}
.list-item-container {
  width: 100%;
  margin: 0 auto;
}
.add-controls {
  width: 100%;
  height: 5vh;
  display: flex;
  justify-content: space-between;
}
.add-input {
  width: 62%;
  margin-bottom: 20px;
  height: 100%;
  padding: 0;
  padding-left: 5px;
  border: none;
  border-bottom: 2px solid  black;
  transition: border .5s ease;
}
.add-input:focus {
  border-bottom: 2px solid #00ab00;
}
.add-button {
   width: 17%;
   height: 100%;
   background: #1dac18;
   color: white;
   display: flex;
   justify-content: center;
   align-items: center;
   font-size: 1.1rem;
   border-radius: 6px;
  transition: background .5s ease;
  font-weight: bold;
 }
.filter-button {
  width: 15%;
  height: 100%;
  background: #ff4938;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.1rem;
  border-radius: 6px;
  font-weight: bold;
  transition: background .5s ease;
}
.filter-button:hover{
  cursor: pointer;
  background: #d03025;
  transition: background .5s ease;
}
.add-button:hover {
  cursor: pointer;
  background: #1a7e0d;
}
.search-panel {
  width: 100%;
  height: 5vh;
  display: flex;
  justify-content: space-between;
  margin-top: 15px;
}
.search-input {
  width: 82%;
  margin-bottom: 20px;
  height: 100%;
  border: none;
  border-bottom: 2px solid  black;
  padding: 0;
  padding-left: 5px;
  transition: border .5s ease;
}
.search-input:focus {
  border-bottom: 2px solid #00ab00;
}
.cansel-search-button {
  width: 12%;
  height: 100%;
  background: #ff493d;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.3rem;
  border-radius: 6px;
  transition: background .5s ease;
}
.cansel-search-button:hover {
  cursor: pointer;
  background: #d03025;
}

@media screen and (max-width: 500px){
  .add-button {
    font-size: .9rem;
  }
  .filter-button {
    font-size: .9rem;
  }
}
</style>
