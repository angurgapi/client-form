<template>
<div class="container">

    <header class='form-header'>
        <h1 class="title">Регистрация клиента</h1>        
    </header>

  <form class="user-form" @submit.prevent="submitForm"> 
    <section>
    <h2 class="section-title">Персональная информация</h2>

    <div class='form-group'>
        <div class="form-field">
            <label id='lastname' class="label-required" for='lastname'>Фамилия</label>
            <input type="text"
            name="lastname"
            @blur="$v.user.lastname.$touch()"
            v-model.trim="user.lastname"
            class="form-input">
        </div>
        <div class="warn">
            <p class="error-message" v-if="$v.user.lastname.$dirty && !$v.user.lastname.required">{{requiredMessage}}</p>
            <p class="error-message" v-if="!$v.user.lastname.alpha">{{ alphaMessage }}</p>
        </div>
    </div>
    
    <div class='form-group'>
        <div class="form-field">
            <label id='firstname' class="label-required" for='firstname'>Имя</label>
            <input type="text"
            name="firstname"
            @blur="$v.user.firstname.$touch()"
            v-model.trim="user.firstname"
            class="form-input">
        </div>
        <div class="warn">
            <p class="error-message" v-if="$v.user.firstname.$dirty && !$v.user.firstname.required">{{requiredMessage}}</p>
            <p class="error-message" v-if="!$v.user.firstname.alpha">{{ alphaMessage }}</p>
        </div>
    </div>

    <div class='form-field'>
        <label id='patronym' for='patronym'>Отчество</label>
        <input type="text" name="patronym" class="form-input">
    </div>

    <div class="form-group">
        <div class='form-field'>
            <label id='birthdate' class="label-required" for='birthdate'>Дата рождения</label>
            <input
                name="birthdate"
                class="form-input"
                id="birthdate"
                @blur="$v.user.birthdate.$touch()"              
                type="date"
                v-model.trim="user.birthdate">
        </div>
        <div class="warn">
            <p class="error-message" v-if="$v.user.birthdate.$dirty && !$v.user.birthdate.required">{{requiredMessage}}</p>
        </div>
    </div>
    <div class="form-group">
     <div class="form-field">
        <label class="label-required" for="phone">Номер телефона</label>    
            <input
                id="phone"
                @blur="$v.user.phone.$touch()"
                type="tel"
                class="form-input"
                v-model.trim="user.phone">
              </div>
              <div class="warn">
                  <p class="error-message" v-if="$v.user.phone.$dirty && (!$v.user.phone.minLength || !$v.user.phone.maxLength ||!$v.user.phone.numeric)">
                    Введите номер телефона из 11 цифр
                  </p>
                  <p class="error-message" v-if="$v.user.phone.$dirty && !$v.user.phone.required">{{requiredMessage}}</p>
                   <p class="error-message" v-if="!$v.user.phone.alphaTel">Телефон должен начинаться с цифры 7</p>
              </div>           
          
    </div>

    <div class='form-field'>
        <label for="gender">Пол</label>
        <div class="form-gender">
            <label>
            <input name='gender' value='male' type="radio" class='radio-option'>Мужской
            </label>
            <label>
            <input name='gender' value='female' type="radio" class='radio-option'>Женский
            </label> 
        </div>         
    </div>

     <div class='form-field'>
        <label id='group' for="doctor">Группа пациентов</label>
        <select multiple id="dropdown" name="group" class="form-input" v-model="user.group">
          <option value='vip'>VIP</option>
          <option value='trouble'>Проблемные</option>
          <option value='insurance'>ОМС</option>       
        </select>
    </div>

    <div class='form-field'>
        <label id='doctor' for="doctor">Лечащий врач</label>
        <select id="dropdown" v-model="user.doctor" name="doctor" class="form-input">
          <option class="form-option" value="ivanov">Иванов</option>
          <option class="form-option" value="zakharov">Захаров</option>
          <option class="form-option" value="chernysheva">Чернышева</option>       
        </select>
    </div>

    <div class='form-field notify'>
    <div class="form-sms"> 
        <label for="sms">Не отправлять СМС</label>
        <input type="checkbox" id="sms" v-model="user.nosms">
    </div>
    </div>
    </section>

    <section>
    <h2 class="section-title">Адрес</h2>    

  
        <div class="form-group">        
            <div class='form-field'>
                <label for='postalcode'>Индекс</label>       
                <input 
                    type="text"
                    @blur="$v.user.postalcode.$touch()"
                    v-model.trim="user.postalcode"
                    name="postalcode"   
                    class="form-input">           
            </div> 
            <div class="warn">
                <p class="error-message" v-if="!$v.user.postalcode.numeric">Индекс может содержать только цифры</p>
            </div>
        </div>

        <div class='form-field'>            
            <label for='country'>Страна</label>       
            <input 
                type="text"
                @blur="$v.user.country.$touch()"
                v-model.trim="user.country"
                class="form-input"
                name="country">                
        </div>
        



    <div class='form-field'>
        <label for='region'>Область</label>       
        <input 
            type="text"
            v-model.trim="user.region"
            name="region"   
            class="form-input">                
    </div> 
    <div class="form-group">
        <div class='form-field'>
            <label class="label-required" for='city'>Город</label>       
            <input 
                type="text"
                @blur="$v.user.city.$touch()"            
                v-model.trim="user.city"
                class="form-input"
                name="city">
        </div> 
        <div class="warn" v-if="$v.user.city.$dirty && !$v.user.city.required">
            <p class="error-message">{{requiredMessage}}</p>
        </div>                 
       
    </div>

    <div class='form-field'>
        <label for='street'>Улица</label>       
        <input 
            type="text"           
            v-model.trim="user.street"
            name="street"   
            class="form-input">               
    </div> 

    <div class='form-field'>
        <label for='country'>Дом</label>       
        <input 
            type="text"               
            v-model.trim="user.building"
            class="form-input"
            name="building">             
     </div>

    </section>

    <section>
    <h2 class="section-title">Документ</h2>

    <div class="form-group">
        <div class='form-field'>
            <label id='document' class="label-required  " for="document">Тип документа</label>
            <select id="document" name="document" v-model.trim="user.documentType" class="form-input">
            <option value="passport">Паспорт</option>
            <option value="certificate">Свидетельство о рождении</option>
            <option value="licence">Вод. удостоверение</option>       
            </select>
        </div>
        <div class="warn">
            <p class="error-message" v-if="$v.user.documentType.$dirty && !$v.user.documentType.required">{{requiredMessage}}</p>
        </div>
    </div>
    <div class="form-field">
        <label for="series">Серия</label>       
        <input 
            type="text"
            name="series"            
            v-model.trim="user.docSeries"             
            class="form-input">               
    </div> 

    <div class="form-group">
        <div class="form-field">
            <label for="doc-number">Номер</label>       
            <input 
                type="text"
                name="doc-number"         
                v-model.trim="user.docNumber"                 
                class="form-input">               
        </div> 
        <div class="warn">
            <p class="error-message" v-if="!$v.user.docNumber.numeric">{{numericMessage}}</p>
        </div>         
    </div>

    <div class='form-field'>
        <label for='issuer'>Кем выдан</label>       
        <input 
            type="text"           
            v-model.trim="user.issuer"
            name="issuer"   
            class="form-input">               
    </div> 

    <div class="form-group">
        <div class="form-field">
            <label class="label-required" for="issueDate">Дата выдачи</label>     
                <input
                    name="issueDate"
                    class="form-input"
                    id="issueDate"
                    @blur="$v.user.issueDate.$touch()"              
                    type="date"
                    v-model.trim="user.issueDate">
            </div>
            <div class="warn">
                <p class="error-message" v-if="$v.user.issueDate.$dirty && !$v.user.issueDate.required">{{requiredMessage}}</p>
            </div>            
    </div>

    </section>

    <div class='form-field'>
        <button type="submit" class='submit-button'>Регистрация</button>
    </div>

  </form>
  </div>
</template>


<script>
import { helpers, required, minLength, maxLength, numeric } from "vuelidate/lib/validators"
const alpha = helpers.regex('alpha', /^[a-zA-Zа-яёА-ЯЁ]*$/)
const alphaTel = helpers.regex('alpha', /^7\d{10}$/);

export default {
    name: 'Form',
    data(){
        return{
            alphaMessage: "Это поле может содержать только буквы",
            numericMessage: "Это поле может содержать только цифры",
            requiredMessage: "Это обязательное поле",
            user:{
                firstname: "",
                lastname: "",
                phone: "",
                birthdate: "",
                group: [],
                doctor: "",
                nosms: false,
                postalcode: "",
                country: "",
                region: "",
                city: "",
                street: "",
                building: "",
                documentType: "",
                issueDate: "",
                docSeries: "",
                docNumber: "",
                issuer: ""         
            }
               
        }
    },
    validations() {
		return {
            user:{
                phone: { minLength: minLength(11), maxLength: maxLength(11), numeric, alphaTel, required },               
                lastname: { required, alpha },
                firstname: { required, alpha },
                postalcode: {numeric},
                country: {alpha},
                city: {required},
                documentType: {required},
                birthdate: {required},
                issueDate: {required},
                docNumber: {numeric}
            }            
        }
	},
    methods: {
        submitForm(){
           this.$v.user.$touch()
            if (!this.$v.user.$invalid) {
                console.log('Успех')
                this.$parent.validated = true           
            }
            else{             
                console.log(this.$v.user.$error)                
            }
        }
    }
}
</script>



<style lang="sass">

*, *::before, *::after
    box-sizing: border-box


body 
    font-family: 'Open Sans', sans-serif
    font-size: 18px
    font-weight: 400
    line-height: 1.4
    margin: 0

.container
    width: 100%
    text-align: center 

.title
    color: rgba(27, 27, 50, 0.8)
.section-title
    margin-top: 30px
    color: rgba(27, 27, 50, 0.6)
    
form
    margin: 0 auto 30px auto
    max-width: 550px
    padding: 2rem 1rem
    border-radius: 0.25rem
    box-shadow: 0 0 10px #333
    background: linear-gradient(90deg, rgba(226,225,246,1) 0%, rgba(218,229,249,1) 54%, rgba(250,251,251,1) 100%)

.form-group
  margin-bottom: 10px
  width: 100%
  text-align: left

.form-field
    display: flex
    flex-direction: row
    justify-content: space-between
.form-sms
    display: flex
    flex-direction: row
.notify
    justify-content: flex-end
label
    display: flex
    align-items: center
    color: rgba(5,9,36,.8)
    font-size: 1.125rem
    font-weight: 600

.label-required::after 
    position: relative
    content: '*'
    color: #611812
    left: 5px

input[type="checkbox"]
    width: 20px
    height: 20px
    margin-left: 10px 

.form-input, .form-gender
    width: 300px
    min-height: 2.375rem
    padding: 0.375rem 0.75rem
    color: #495057    
    border-radius: 0.25rem
    margin-top: 10px
    margin-bottom: 10px

.form-input
    font-size: 18px
    background-color: #fff
    transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out
    border: 1px solid #ced4da
.radio-option, .check
  margin-right: 7px

p
  font-size: 1.125rem
  color: #1b1b32

.error-message
    color: #611812
    font-size: 14px
    text-align: right
    margin-top: 0

.submit-button
    width: 200px
    height: 50px
    margin: auto
    background-color: rgba(5,9,36,.8)
    color: rgba(255,255,255,.8)
    border-radius: 10px
    cursor: pointer
    padding: 0.75rem
    font-weight: 700
    line-height: 1.4
    font-size: 18px
    box-shadow: 0 0 50px rgba(0,0,0,.4)
    letter-spacing: 2px

@media(max-width: 500px)
    .form-field
        flex-direction: column
    .form-input
        width: 100%
</style>