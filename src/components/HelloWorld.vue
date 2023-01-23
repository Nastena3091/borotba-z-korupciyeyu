<script>
import { ref } from 'vue'
import MsgSend from "./MsgSend.vue";
import  institut from "../data/institution"
import types from "../data/types"
import areas from "../data/areas"
export default{
  data(){
    return{
      institutions:[],
      types: [],
      areas: [],
      institution : '',
      type:'',
      area:'',
      position:'',
      teacher:'',
      offense:'',
      student:'',
      adress:'',
      msg:'',
      inputSingly:false,
      anonimus: false,
      isOpen:false
    }
  },
  components: { MsgSend },
  setup() {
    const isOpen = ref(false);
    return { isOpen };
  },
  mounted(){
    this.areas=areas
  },
  methods:{
    selectInstitution(){
      this.institutions=institut
      console.log(this.type)
      console.log(this.area)
      this.institutions=this.institutions.filter((elem)=>{
        return elem.type==this.type&&elem.area==this.area
      })
    },
    selectType(){
      this.types=types
    },
    sendEmail(){
      console.log(this.institution)
      console.log(this.position)
      console.log(this.teacher)
      console.log(this.offense)
      console.log(this.student)
      console.log(this.adress)
      if(!this.anonimus){
        if(this.student && this.institution&&this.position&&this.teacher&&this.offense&&this.adress){
          emailjs.send("service_vpjca4j","template_yfjgoe4",{
            institution: this.institution,
            position: this.position,
            teacher: this.teacher,
            offense: this.offense,
            student: this.student,
            address: this.adress,
            to_email: "sladkovaoe@gmail.com"
          })
          this.msg="Ваш лист успішно надіслан!"
        }else this.msg="Не всі поля заповлені."
      } else{
        if(this.institution&&this.position&&this.teacher&&this.offense){
          emailjs.send("service_vpjca4j","template_12y0r47",{
            institution: this.institution,
            position: this.position,
            teacher: this.teacher,
            offense: this.offense,
            to_email:"sladkovaoe@gmail.com",
          })
          this.msg="Ваш лист успішно надіслан!"
        }else this.msg="Не всі поля заповлені."
      }
      this.isOpen = true
      this.area='',
      this.type='',
      this.institution=''
      this.position=''
      this.teacher=''
      this.offense=''
      this.student=''
      this.adress=''
    }
  }
}
</script>

<template>
  <main>
    <h1>ПОВІДОМИТИ ПРО КОРУПЦІЮ</h1>
    <input type="checkbox" v-model="inputSingly">Ввести самостійно навчальний заклад
    <div id="institution" v-if="!inputSingly">
      <p>Вищій навчальний заклад:</p>
      <select id="areas" v-model="area" @change="selectType">
        <option value="">Регіон</option>
        <option :value="a" v-for="a in areas" :key="a">{{a}}</option>
      </select>
      <select id="types" v-model="type" @change="selectInstitution">
        <option value="">Тип установи</option>
        <option :value="t" v-for="t in types" :key="t">{{t}}</option>
      </select>
      <select id="institutions" v-model="institution" >
        <option value="">Назва установи</option>
        <option :value="i.fullName" v-for="i in institutions" :key="i.id" style="width: 500px">{{i.fullName}}</option>
      </select>
    </div>
    <div v-else>
      <p>Навчальний заклад:</p>
      <input type="text" v-model.lazy="institution">
    </div>
    <div id="teacher">
      <p>Посада</p>
      <input type="text" v-model.lazy="position">
      <p>Прізвище, ім'я, по-батькові:</p>
      <input type="text" v-model.lazy="teacher">
    </div>
    <div id="offense">
      <p>Правопорушення:</p>
      <textarea cols="30" rows="10" v-model="offense"></textarea>
    </div>
    <input type="checkbox" v-model="anonimus">Анонімно <span v-if="anonimus">(<i>Анонімне повідомлення про порушення вимог антикорупційного законодавства підлягає розгляду, якщо зазначена в ньому інформація стосується конкретної особи, містить фактичні дані, які можуть бути перевірені</i>.)</span>
    <div id="student" v-else>
      <p>Прізвище, ім'я, по-батькові:</p>
      <input type="text" v-model.lazy="student">
      <p>Адреса (область, населений пункт, вулиця, будинок):</p>
      <input type="text" v-model.lazy="adress">
    </div>
    <input type="button" @click="sendEmail" value="Відправити">

    <MsgSend :open="isOpen">
      <h1>{{ msg }}</h1>
      <input type="button" value="Закрити" @click="isOpen = !isOpen">
    </MsgSend>
  </main>
  
</template>

<style scoped>

</style>
