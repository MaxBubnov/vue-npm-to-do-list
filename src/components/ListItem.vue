<template>
    <div class="list-item">
        <div class="item-inner-text" @click="doneMail" @dblclick="changing = !changing">
            <div class="text-span" v-if="!changing" :class='{"red": importantTitle && !isDone,"done": isDone}'>{{ currentText }}</div>
            <div class="deadline-text">{{ deadlineProp.days + '  :   ' + deadlineProp.hours + '  :   ' + deadlineProp.minutes }}</div>
            <input type="text" v-if="changing" class="change-input" @keydown.enter="changeText" placeholder="Replaced by..." v-model="currentText">
        </div>
        <div class="item-buttons-block">
            <div class="item-button important-button" @click="importantMail">!</div>
            <div class="item-button delete-button" @click="deleteMail"><i class="far fa-trash-alt"></i></div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'ListItem',
  props: {
      inputText: String,
      identyfyNumber: Number,
      importantProp: Boolean,
      doneProp: Boolean,
      deadlineProp: Object
  },
  data: function () {
   return{
       importantTitle: this.importantProp,
       isDone: this.doneProp,
       changing: false,
       currentText: this.inputText,
       //currentDeadline: this.deadlineProp
   }
  },
    methods: {
      deleteMail() {
          this.$emit('deleteItem')
      },
      importantMail(){
          this.importantTitle = !this.importantTitle;
          this.$emit('importantChange')
          console.log('deadlineProp:    ',this.deadlineProp);
      },
      doneMail(){
            this.isDone = !this.isDone;
            this.$emit('doneChange')
      },
      changeText(){
          this.$emit('itemTextChanging', {
              changedText: this.currentText,
              identForTextChanging: this.identyfyNumber
          });
          this.isDone = false;
          this.changing = !this.changing;
      }
    }
}
</script>
<style>
    .list-item {
        width: 100%;
        height: 7vh;
        display: flex;
        justify-content: space-between;
        margin: 6px 0;
    }
    .item-inner-text {
        width: 84%;
        height: 100%;
        position: relative;
        display: flex;
        justify-content: space-between;
        align-items: center;
        font-size: 1.3rem;
        overflow: hidden;
        transition: all .5s ease;
    }
    .item-buttons-block {
        width: 15%;
        display: flex;
        justify-content: flex-end;
        align-items: center;
    }
    .item-button {
        width: 48%;
        height: 85%;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 1.15rem;
        border-radius: 6px;
        color: white;
        text-decoration: none;
    }
    .item-button:hover {
        cursor: pointer;
    }
    .important-button{
        background: #ff4938;
        box-shadow: 0px 0px 1.5px #ff4938;
        font-weight: bolder;
    }
    .delete-button{
        background: #00c100;
        margin-left: .5vw;
        box-shadow: 0 0 1.5px #00c100;
    }
    .red {
        color: #ff4938!important;
        font-weight: bold;
    }
    .done::before{
        content: '';
        width: 100%!important;
    }
    .text-span{
         height: 100%;
         /*position: absolute;*/
         display: flex;
         justify-content: flex-start;
         align-items: center;
         color: #2c3e50;
         transition: color .7s ease;
     }
    .text-span:hover{
        cursor: pointer;
    }
    .text-span::before{
        content: '';
        display: block;
        height: 3px;
        position: absolute;
        background: black;
        width: 0;
        transition: width .8s ease-in-out;
    }
    .change-input {
        width: 95%;
        height: 95%;
        font-size: 1rem;
        border: none;
        border-bottom: 2px solid  black;
        padding: 0;
        padding-left: 5px;
        outline: none;
        transition: border .5s ease;
    }
    .change-input:focus {
        border-bottom: 2px solid #00ab00;
    }
    .deadline-text {
        color: #d91c1d;
    }
    @media screen and (max-width: 500px){
        .text-span{
            font-size: .9rem;
        }
        .item-buttons-block {
            width: 25%;
            display: flex;
            justify-content: flex-end;
            align-items: center;
        }
        .text-span::before{
            content: '';
            height: 2px;
        }
    }
</style>
