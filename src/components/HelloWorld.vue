<script>
import { ref } from 'vue'
import  institut from "../data/institution"
import typ from "../data/types"
import are from "../data/areas"
import MsgSend from "./MsgSend.vue"

export default {
  components: { MsgSend },
  setup() {
    var isOpen = ref(false),
          institutions=ref([]),
          types=typ,
          areas=are,
          institution=ref(''),
          type=ref(''),
          area=ref(''),
          position=ref(''),
          teacher=ref(''),
          offense=ref(''),
          student=ref(''),
          adress=ref(''),
          msg=ref(''),
          inputSingly=ref(false),
          anonimus=ref(false)
    
    function selectInstitution(){
      institutions.value=institut.filter((elem)=>{
        return elem.type==type.value&&elem.area==area.value
      })
      console.log(institutions)
    }
    function sendEmail(){
      if(!anonimus.value){
        if(student.value && institution.value&&position.value&&teacher.value&&offense.value&&adress.value){
          emailjs.send("service_vpjca4j","template_yfjgoe4",{
            institution: institution.value,
            position: position.value,
            teacher: teacher.value,
            offense: offense.value,
            student: student.value,
            address: adress.value,
            to_email: "nastena20043091@gmail.com"
          })
          msg.value="Ваш лист успішно надіслан!"
        }else msg.value="Не всі поля заповлені."
      } else{
        if(institution.value&&position.value&&teacher.value&&offense.value){
          emailjs.send("service_vpjca4j","template_12y0r47",{
            institution: institution.value,
            position: position.value,
            teacher: teacher.value,
            offense: offense.value,
            to_email:"nastena20043091@gmail.com",
          })
          msg.value="Ваш лист успішно надіслан!"
        }else msg.value="Не всі поля заповлені."
      }
      isOpen.value = true
      area.value='',
      type.value='',
      institution.value=''
      position.value=''
      teacher.value=''
      offense.value=''
      student.value=''
      adress.value=''
    }
    return { isOpen, institutions, types,areas,institution,type,area,position,teacher,offense,student,adress,msg,inputSingly,anonimus, selectInstitution, sendEmail};
    
  },
}
</script>

<template>
  <main>
    <h1>ПОВІДОМИТИ ПРО КОРУПЦІЮ</h1>
    <input type="checkbox" v-model="inputSingly">Ввести самостійно навчальний заклад
    <div id="institution" v-if="!inputSingly">
      <p>Вищій навчальний заклад:</p>
      <select id="areas" v-model="area">
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
