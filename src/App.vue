<template>
  <div class="header">
    <ul class="header-button-left">
      <li @click="back_step" v-if="step==1 || step==2">Cancel</li>
    </ul>
    <ul class="header-button-right">
      <li v-if="step==1" @click="next_step">Next</li>
      <li v-if="step==2" @click="publish">OK</li>
    </ul>
    <img src="./assets/logo.png" class="logo" />
  </div>

  <Container :vue_data="vue_data" :step="step" :img_url="img_url"  @writeee="content = $event"/>

  <button @click="more" v-show="more_show == true">More</button>

  <div class="footer">
    <ul class="footer-button-plus">
      <input @change="upload" type="file" id="file" class="inputfile" />
      <label for="file" class="input-plus">➕</label>
    </ul>
 </div>

<!-- <div style="margin:100px 0 100px 0;">
  <button @click="step=0">button0</button>
  <button @click="step=1">button1</button>
  <button @click="step=2">button2</button>
  <div v-if="step == 0">contents0</div>
  <div v-if="step == 1">contents1</div>
  <div v-if="step == 2">contents2</div>
</div> -->

</template>

<script>
import './assets/style.css';
import Container from './components/Container.vue'
import vue_data from './vuestadata.js';
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      vue_data: vue_data,
      click_count: 0,
      step: 0,
      more_show: true,
      img_url : '',
      content: '',
    }
  },
  components: {
    Container : Container,
  },
  methods: {

    more() {
      axios.get(`https://codingapple1.github.io/vue/more${this.click_count}.json`)
      .then((result)=> {
        console.log(result.data);
        this.vue_data.push(result.data); //data추가
        this.click_count++;
        console.log(this.vue_data); //data추가된거 확인
        console.log(this.click_count);
      }).catch((error)=>{
        console.log(error);
        this.more_show=false;
      })  
    },

    upload(e) {
      let file = e.target.files
      console.log(file[0].type);
      let url = URL.createObjectURL(file[0]);
      console.log(url);
      this.step=1;
      this.img_url = url;
      console.log(this.img_url);
    },

    back_step() {
      if(this.step==1){
        this.step=0;
      }else if(this.step==2){
        this.step=1;
      }
    },

    next_step() {
      if(this.step==1){
        this.step=2
      }
    },

    publish() {
      var new_data = {
      name: "zinkki_z",
      userImage: "https://placeimg.com/200/200/people",
      postImage: `${this.img_url}`,
      likes: 20,
      date: "Oct 06",
      liked: false,
      content: this.content,
      filter: "clarendon",
			num: 1
    };
      this.vue_data.unshift(new_data);
      this.step=0;
    },

  },
}
</script>

<style>
body {
  margin: 0;
}
ul {
  padding: 5px;
  list-style-type: none;
}
.logo {
  width: 22px;
  margin: auto;
  display: block;
  position: absolute;
  left: 0;
  right: 0;
  top: 13px;
}
.header {
  width: 100%;
  height: 40px;
  background-color: white;
  padding-bottom: 8px;
  position: sticky;
  top: 0;
}
.header-button-left {
  color: skyblue;
  float: left;
  width: 50px;
  padding-left: 20px;
  cursor: pointer;
  margin-top: 10px;
}
.header-button-right {
  color: skyblue;
  float: right;
  width: 50px;
  cursor: pointer;
  margin-top: 10px;
}
.footer {
  width: 100%;
  position: sticky;
  bottom: 0;
  padding-bottom: 10px;
  background-color: white;
}
.footer-button-plus {
  width: 80px;
  margin: auto;
  text-align: center;
  cursor: pointer;
  font-size: 24px;
  padding-top: 12px;
}
.sample-box {
  width: 100%;
  height: 600px;
  background-color: bisque;
}
.inputfile {
  display: none;
}
.input-plus {
  cursor: pointer;
}
</style>
