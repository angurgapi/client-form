<template>
<div class="container">
    <form v-if="!isCreated" class="form" @submit.prevent="checkForm">
      <div class="form__box">
        <h2 class="form__title form__title_profile">Поля профиля</h2>
        <fieldset class="form__group">
          <legend class="visually-hidden">Поля профиля</legend>
          <div class="form__field">
              <label class="form__label form__label_required" for="surname">Фамилия</label>
              <div class="form__field-wrap">
                <input
                  id="surname"
                  @blur="$v.formCreateUser.surname.$touch()"
                  type="text"
                  :class="$v.formCreateUser.surname.$error ? 'is-invalid' : ''"
                  v-model.trim="formCreateUser.surname"
                >
                <div class="form__errors">
                  <p class="invalid-feedback" v-if="$v.formCreateUser.surname.$dirty && !$v.formCreateUser.surname.required">Введите фамилию</p>
                  <p class="invalid-feedback" v-if="$v.formCreateUser.surname.$dirty && !$v.formCreateUser.surname.alpha">{{ alphaText }}</p>
                </div>
              </div>
          </div>
          <div class="form__field">
              <label class="form__label form__label_required" for="name">Имя</label>
              <div class="form__field-wrap">
                <input
                  id="name"
                  @blur="$v.formCreateUser.name.$touch()"
                  type="text"
                  :class="$v.formCreateUser.name.$error ? 'is-invalid' : ''"
                  v-model.trim="formCreateUser.name"
                >
                <div class="form__errors">
                  <p class="invalid-feedback" v-if="$v.formCreateUser.name.$dirty && !$v.formCreateUser.name.required">Введите имя</p>
                  <p class="invalid-feedback" v-if="$v.formCreateUser.name.$dirty && !$v.formCreateUser.name.alpha">{{ alphaText }}</p>
                </div>
            </div>
          </div>
          <div class="form__field">
            <label class="form__label" for="patronymiс">Отчество</label>
            <div class="form__field-wrap">
              <input
                id="patronymiс"
                @blur="$v.formCreateUser.patronymic.$touch()"
                type="text"
                :class="$v.formCreateUser.patronymic.$error ? 'is-invalid' : ''"
                v-model.trim="formCreateUser.patronymic"
              >
              <div class="form__errors">
                  <p class="invalid-feedback" v-if="$v.formCreateUser.patronymic.$dirty && !$v.formCreateUser.patronymic.alpha">{{ alphaText }}</p>
              </div>
            </div>
          </div>
          <div class="form__field">
              <label class="form__label form__label_required" for="birth-date">Дата рождения</label>
              <div class="form__field-wrap form__field-wrap_date">
                <input
                  class="no-icon"
                  id="birth-date"
                  @blur="$v.formCreateUser.dateOfBirth.$touch()"
                  :class="$v.formCreateUser.dateOfBirth.$error ? 'is-invalid' : ''"
                  type="date"
                  v-model.trim="formCreateUser.dateOfBirth"
                >
              <div class="form__errors">
                <p class="invalid-feedback" v-if="$v.formCreateUser.dateOfBirth.$dirty && !$v.formCreateUser.dateOfBirth.required">Введите дату рождения</p>
                <p class="invalid-feedback" v-if="$v.formCreateUser.dateOfBirth.$dirty && (!$v.formCreateUser.dateOfBirth.maxLength || !$v.formCreateUser.dateOfBirth.minLength)">Введите корректную дату</p>
              </div>
            </div>
          </div>
          <div class="form__field">
            <p class="form__label">Пол</p>
              <div class="form__field-wrap form__field-wrap_gendere">
                <div>
                  <input type="radio" value="male" name="exampleRadios" id="male" v-model="formCreateUser.gendere" checked>
                  <label for="male">Мужчина</label>
                </div>
                <div>
                  <input type="radio" value="female" name="exampleRadios" id="female" v-model="formCreateUser.gendere">
                  <label for="female">Женщина</label>
                </div>
            </div>
          </div>
          <div class="form__field">
            <label class="form__label form__label_required" for="phone">Номер телефона</label>
            <div class="form__field-wrap">
              <input
                id="phone"
                @blur="$v.formCreateUser.phoneNumber.$touch()"
                type="tel"
                placeholder="79102546400"
                :class="$v.formCreateUser.phoneNumber.$error ? 'is-invalid' : ''"
                v-model.trim="formCreateUser.phoneNumber"
              >
              <div class="form__errors">
                  <p 
                    class="invalid-feedback" 
                    v-if="$v.formCreateUser.phoneNumber.$dirty && 
                    (!$v.formCreateUser.phoneNumber.minLength || 
                    !$v.formCreateUser.phoneNumber.maxLength ||
                    !$v.formCreateUser.phoneNumber.numeric)"
                  >
                    Телефон должен содержать 11 цифр
                  </p>
                  <p class="invalid-feedback" v-if="$v.formCreateUser.phoneNumber.$dirty && !$v.formCreateUser.phoneNumber.required">Введите номер телефона</p>
              </div>
            </div>
          </div>
          <div class="form__field">
              <label class="form__label" for="doctors">Лечащий врач</label>
              <div class="form__field-wrap">
                <select class="form__select form__select_doctors" id="doctors" v-model="formCreateUser.doctor">
                  <option
                    v-for="(doctor, index) in doctors"
                    :key="index" 
                    :value="doctor.value"
                  >
                  {{ doctor.label }}
                  </option>
                </select>
              </div>
          </div>
          <div class="form__field">
              <label class="form__label form__label_required" for="customer-group">Группа клиентов</label>
              <div class="form__field-wrap form__field-wrap_select">
                <select 
                  class="form__select form__select_group"  
                  :class="$v.formCreateUser.favoriteGroup.$error ? 'is-invalid' : ''"
                  id="customer-group" 
                  v-model="formCreateUser.favoriteGroup" 
                  size="3"
                  multiple
                >
                  <option
                    v-for="(group, index) in customerGroup"
                    :key="index" 
                    :value="group.value"
                  >
                  {{ group.label }}
                  </option>
                </select>
                <div class="form__errors">
                  <p class="invalid-feedback" v-if="$v.formCreateUser.favoriteGroup.$dirty && !$v.formCreateUser.favoriteGroup.required">Выберите одну или несколько групп</p>
                </div>
              </div>
          </div>
            <div class="form__field form__field_notification">
                <input
                  type="checkbox"
                  id="notification"
                  v-model="formCreateUser.agreeWithSendSms"
                >
                <label for="notification">Не отправлять СМС</label>
            </div>
        </fieldset>
      </div>
      <div class="form__box">
        <h2 class="form__title form__title_address">Адрес</h2>
        <fieldset class="form__group">
          <legend class="visually-hidden">Адрес</legend>
          <div class="form__field">
              <label class="form__label" for="post-code">Индекс</label>
              <div class="form__field-wrap">
                <input
                  id="post-code"
                  @blur="$v.formCreateUser.postCode.$touch()"
                  type="text"
                  :class="$v.formCreateUser.postCode.$error ? 'is-invalid' : ''"
                  v-model.trim="formCreateUser.postCode"
                >
                <div class="form__errors">
                  <p class="invalid-feedback" v-if="$v.formCreateUser.postCode.$dirty && (!$v.formCreateUser.postCode.numeric || !$v.formCreateUser.postCode.minLength || !$v.formCreateUser.postCode.maxLength)">Индекс содержит 6 цифр</p>
                </div>
            </div>
          </div>
          <div class="form__field">
              <label class="form__label" for="country">Страна</label>
              <div class="form__field-wrap">
                <input
                  id="country"
                  @blur="$v.formCreateUser.country.$touch()"
                  type="text"
                  :class="$v.formCreateUser.country.$error ? 'is-invalid' : ''"
                  v-model.trim="formCreateUser.country"
                >
                <div class="form__errors">
                  <p class="invalid-feedback" v-if="$v.formCreateUser.country.$dirty && !$v.formCreateUser.country.alpha">{{ alphaText }}</p>
                </div>
            </div>
          </div>
          <div class="form__field">
              <label class="form__label" for="region">Область</label>
              <div class="form__field-wrap">
                <input
                  id="region"
                  @blur="$v.formCreateUser.region.$touch()"
                  type="text"
                  :class="$v.formCreateUser.region.$error ? 'is-invalid' : ''"
                  v-model.trim="formCreateUser.region"
                >
                <div class="form__errors">
                  <p class="invalid-feedback" v-if="$v.formCreateUser.region.$dirty && !$v.formCreateUser.region.alphaModify">{{ alphaText }}</p>
                </div>
            </div>
          </div>
          <div class="form__field">
              <label class="form__label form__label_required" for="city">Город</label>
              <div class="form__field-wrap">
                <input
                  id="city"
                  @blur="$v.formCreateUser.city.$touch()"
                  type="text"
                  :class="$v.formCreateUser.city.$error ? 'is-invalid' : ''"
                  v-model.trim="formCreateUser.city"
                >
                <div class="form__errors">
                  <p class="invalid-feedback" v-if="$v.formCreateUser.city.$dirty && !$v.formCreateUser.city.required">Введите город</p>
                  <p class="invalid-feedback" v-if="$v.formCreateUser.city.$dirty && !$v.formCreateUser.city.alphaModify">{{ alphaText }}</p>
                </div>
              </div>
          </div>
            <div class="form__field">
              <label class="form__label" for="street">Улица</label>
              <div class="form__field-wrap">
                <input
                  id="street"
                  @blur="$v.formCreateUser.street.$touch()"
                  type="text"
                  :class="$v.formCreateUser.street.$error ? 'is-invalid' : ''"
                  v-model.trim="formCreateUser.street"
                >
                <div class="form__errors">
                  <p class="invalid-feedback" v-if="$v.formCreateUser.street.$dirty && !$v.formCreateUser.street.alphaModify">{{ alphaText }}</p>
                </div>
              </div>
          </div>
          <div class="form__field">
              <label class="form__label" for="house">Дом</label>
              <div class="form__field-wrap form__field-wrap_house">
                <input
                  id="house"
                  @blur="$v.formCreateUser.houseNumber.$touch()"
                  type="text"
                  :class="$v.formCreateUser.houseNumber.$error ? 'is-invalid' : ''"
                  v-model.trim="formCreateUser.houseNumber"
                >
                <div class="form__errors">
                  <p class="invalid-feedback" v-if="$v.formCreateUser.houseNumber.$dirty && !$v.formCreateUser.houseNumber.alphaNum">Запрещены спецсимволы</p>
                </div>
              </div>
          </div>
        </fieldset>
      </div>
      <div class="form__box">
        <h2 class="form__title form__title_documents">Документ</h2>
        <fieldset class="form__group">
          <legend class="visually-hidden">Документы</legend>
          <div class="form__field">
              <label class="form__label" for="documents">Тип документа</label>
              <div class="form__field-wrap">
                <select class="form__select form__select_doctors" id="documents" v-model="formCreateUser.document">
                  <option
                    v-for="(document, index) in documents"
                    :key="index" 
                    :value="document.value"
                  >
                  {{ document.label }}
                  </option>
                </select>
              </div>
          </div>
          <div class="form__documents" v-if="formCreateUser.document == 'passport'">
            <div class="form__field">
                <label class="form__label" for="pass-series">Серия</label>
                <div class="form__field-wrap">
                  <input
                    id="pass-series"
                    @blur="$v.formCreateUser.passSeries.$touch()"
                    type="text"
                    :class="$v.formCreateUser.passSeries.$error ? 'is-invalid' : ''"
                    v-model.trim="formCreateUser.passSeries"
                  >
                  <div class="form__errors">
                    <p class="invalid-feedback" v-if="$v.formCreateUser.passSeries.$dirty && (!$v.formCreateUser.passSeries.numeric || !$v.formCreateUser.passSeries.minLength || !$v.formCreateUser.passSeries.maxLength)">Серия содержит 4 цифры</p>
                  </div>
              </div>
            </div>
            <div class="form__field">
                <label class="form__label" for="pass-number">Номер</label>
                <div class="form__field-wrap">
                  <input
                    id="pass-number"
                    @blur="$v.formCreateUser.passNumber.$touch()"
                    type="text"
                    :class="$v.formCreateUser.passNumber.$error ? 'is-invalid' : ''"
                    v-model.trim="formCreateUser.passNumber"
                  >
                  <div class="form__errors">
                    <p class="invalid-feedback" v-if="$v.formCreateUser.passNumber.$dirty && (!$v.formCreateUser.passNumber.numeric || !$v.formCreateUser.passNumber.minLength || !$v.formCreateUser.passNumber.maxLength)">Номер содержит 6 цифры</p>
                  </div>
              </div>
            </div>
            <div class="form__field">
                <label class="form__label" for="issued">Кем выдан</label>
                <div class="form__field-wrap">
                  <textarea 
                    id="issued"
                    @blur="$v.formCreateUser.passIssued.$touch()"
                    class="form__textarea"
                    :class="$v.formCreateUser.passIssued.$error ? 'is-invalid' : ''"
                    v-model.trim="formCreateUser.passIssued"
                  ></textarea>
                  <div class="form__errors">
                    <p class="invalid-feedback" v-if="$v.formCreateUser.passIssued.$dirty && !$v.formCreateUser.passIssued.alphaModify">{{ alphaText }}</p>
                  </div>
              </div>
            </div>
            <div class="form__field">
                <label class="form__label form__label_required" for="pass-date-of-issue">Дата выдачи</label>
                <div class="form__field-wrap form__field-wrap_date">
                  <input
                    class="no-icon"
                    id="pass-date-of-issue"
                    @blur="$v.formCreateUser.passDateOfIssue.$touch()"
                    :class="$v.formCreateUser.passDateOfIssue.$error ? 'is-invalid' : ''"
                    type="date"
                    v-model.trim="formCreateUser.passDateOfIssue"
                  >
                <div class="form__errors">
                  <p class="invalid-feedback" v-if="$v.formCreateUser.passDateOfIssue.$dirty && !$v.formCreateUser.passDateOfIssue.required">Введите дату выдачи</p>
                  <p class="invalid-feedback" v-if="$v.formCreateUser.passDateOfIssue.$dirty && (!$v.formCreateUser.passDateOfIssue.maxLength || !$v.formCreateUser.passDateOfIssue.minLength)">Введите корректную дату</p>
                </div>
              </div>
            </div>
          </div>
          <div class="form__documents" v-if="formCreateUser.document == 'birthСertificate'">
            <div class="form__field">
                <label class="form__label" for="birth-certificate-series">Серия</label>
                <div class="form__field-wrap">
                  <input
                    id="birth-certificate-series"
                    @blur="$v.formCreateUser.birthСertificateSeries.$touch()"
                    type="text"
                    :class="$v.formCreateUser.birthСertificateSeries.$error ? 'is-invalid' : ''"
                    v-model.trim="formCreateUser.birthСertificateSeries"
                  >
                  <div class="form__errors">
                    <p class="invalid-feedback" v-if="$v.formCreateUser.birthСertificateSeries.$dirty && (!$v.formCreateUser.birthСertificateSeries.minLength || !$v.formCreateUser.birthСertificateSeries.maxLength)">Серия содержит 4 символа</p>
                  </div>
              </div>
            </div>
            <div class="form__field">
                <label class="form__label" for="birth-certificate-number">Номер</label>
                <div class="form__field-wrap">
                  <input
                    id="birth-certificate-number"
                    @blur="$v.formCreateUser.birthСertificateNumber.$touch()"
                    type="text"
                    :class="$v.formCreateUser.birthСertificateNumber.$error ? 'is-invalid' : ''"
                    v-model.trim="formCreateUser.birthСertificateNumber"
                  >
                  <div class="form__errors">
                    <p class="invalid-feedback" v-if="$v.formCreateUser.birthСertificateNumber.$dirty && (!$v.formCreateUser.birthСertificateNumber.numeric || !$v.formCreateUser.birthСertificateNumber.minLength || !$v.formCreateUser.birthСertificateNumber.maxLength)">Номер содержит 6 цифры</p>
                  </div>
              </div>
            </div>
            <div class="form__field">
                <label class="form__label" for="bc-date-of-issue">Дата выдачи</label>
                <div class="form__field-wrap form__field-wrap_date">
                  <input
                    class="no-icon"
                    id="bc-date-of-issue"
                    @blur="$v.formCreateUser.birthСertificateDateOfIssue.$touch()"
                    :class="$v.formCreateUser.birthСertificateDateOfIssue.$error ? 'is-invalid' : ''"
                    type="date"
                    v-model.trim="formCreateUser.birthСertificateDateOfIssue"
                  >
                <div class="form__errors">
                  <p class="invalid-feedback" v-if="$v.formCreateUser.birthСertificateDateOfIssue.$dirty && (!$v.formCreateUser.birthСertificateDateOfIssue.maxLength || !$v.formCreateUser.birthСertificateDateOfIssue.minLength)">Введите корректную дату</p>
                </div>
              </div>
            </div>
            <div class="form__field">
                <label class="form__label" for="registration-place">Место государственной регистрации</label>
                <div class="form__field-wrap">
                  <textarea 
                    id="registration-place"
                    @blur="$v.formCreateUser.placeOfRegistration.$touch()"
                    class="form__textarea"
                    :class="$v.formCreateUser.placeOfRegistration.$error ? 'is-invalid' : ''"
                    v-model.trim="formCreateUser.placeOfRegistration"
                  ></textarea>
                  <div class="form__errors">
                    <p class="invalid-feedback" v-if="$v.formCreateUser.placeOfRegistration.$dirty && !$v.formCreateUser.placeOfRegistration.alphaModify">{{ alphaText }}</p>
                  </div>
              </div>
            </div>
          </div>
           <div class="form__documents" v-if="formCreateUser.document == 'driverLicense'">
            <div class="form__field">
                <label class="form__label" for="driver-license-number">Номер</label>
                <div class="form__field-wrap">
                  <input
                    id="driver-license-number"
                    @blur="$v.formCreateUser.driverLicenseNumber.$touch()"
                    type="text"
                    :class="$v.formCreateUser.driverLicenseNumber.$error ? 'is-invalid' : ''"
                    v-model.trim="formCreateUser.driverLicenseNumber"
                  >
                  <div class="form__errors">
                    <p class="invalid-feedback" v-if="$v.formCreateUser.driverLicenseNumber.$dirty && (!$v.formCreateUser.driverLicenseNumber.numeric || !$v.formCreateUser.driverLicenseNumber.minLength || !$v.formCreateUser.driverLicenseNumber.maxLength)">Номер содержит 10 цифр</p>
                  </div>
              </div>
            </div>
          </div>
        </fieldset>
      </div>
      <div class="form__buttons">
            <button class="form__button_reset button button_reset" type="button" @click="resetForm">Отмена</button>
            <button class="form__button_register button button_register" type="submit">Зарегистрироваться</button>
      </div>
    </form>
    
</div>
</template>

<script>

import { required, helpers, minLength, maxLength, numeric } from 'vuelidate/lib/validators'
const alpha = helpers.regex('alpha', /^[a-zA-Zа-яёА-ЯЁ]*$/);
const alphaModify = helpers.regex('alphaModify', /^[a-zA-Zа-яёА-ЯЁ_ -]*$/);
const alphaNum = helpers.regex('alpha', /^[a-zA-Zа-яёА-ЯЁ0-9]*$/);
export default {
  name: 'FormPage',
  data() {
    return {
      alphaText: 'Запрещены цифры и другие символы',
      numericText: 'Запрещены буквы и другие символы',
      isCreated: false,
      formCreateUser: {
        name: '',
        surname: '',
        patronymic: '',
        dateOfBirth: '',
        gendere: 'male',
        phoneNumber: '',
        doctor: 'Ivanov',
        favoriteGroup: [],
        agreeWithSendSms: false,
        postCode: '',
        country: '',
        city: '',
        street: '',
        houseNumber: '',
        document: 'passport',
        passSeries: '',
        passNumber: '',
        passIssued: '',
        passDateOfIssue: '',
        birthСertificateSeries: '',
        birthСertificateNumber: '',
        birthСertificateDateOfIssue: '',
        placeOfRegistration: '',
        driverLicenseNumber: ''
      },
      doctors: [
        { label: 'Иванов', value: 'Ivanov' },
        { label: 'Захаров', value: 'Zaharov' },
        { label: 'Черышева', value: 'Cherisheva' }
      ],
      customerGroup: [
        { label: 'VIP', value: 'Vip' },
        { label: 'Проблемные', value: 'Problem' },
        { label: 'ОМС', value: 'OMS' }
      ],
      documents: [
        { label: 'Паспорт', value: 'passport' },
        { label: 'Свидетельство о рождении', value: 'birthСertificate' },
        { label: 'Вод. удостоверение', value: 'driverLicense' }
      ]
    }
  },

  validations: {
    formCreateUser: {
      surname: { required, alpha },
      name: { required, alpha },
      patronymic: { alpha },
      favoriteGroup: { required },
      dateOfBirth: { required, minLength: minLength(10), maxLength: maxLength(10) },
      phoneNumber: { minLength: minLength(11), maxLength: maxLength(11), numeric, required },
      postCode: { numeric, minLength: minLength(6), maxLength: maxLength(6) },
      country: { alpha },
      region: { alphaModify },
      city: { required, alphaModify },
      street: { alphaModify },
      houseNumber: { alphaNum },
      passSeries: { numeric, minLength: minLength(4), maxLength: maxLength(4) },
      passNumber: { numeric, minLength: minLength(6), maxLength: maxLength(6) },
      passIssued: { alphaModify },
      passDateOfIssue: { required, minLength: minLength(10), maxLength: maxLength(10) },
      birthСertificateSeries: { minLength: minLength(4), maxLength: maxLength(4) },
      birthСertificateNumber: { numeric, minLength: minLength(6), maxLength: maxLength(6) },
      birthСertificateDateOfIssue: { minLength: minLength(10), maxLength: maxLength(10) },
      placeOfRegistration: { alphaModify },
      driverLicenseNumber: { numeric, minLength: minLength(10), maxLength: maxLength(10) }
    }
  },
  methods: {
    checkForm() {
      this.$v.formCreateUser.$touch();
      if (!this.$v.formCreateUser.$error) {
        console.log('Успех');
        this.isCreated = true;
      }
    },
    resetForm() {
      window.location.reload();
    }
  }
}
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;600;700&display=swap');
  body {
    font-family: 'Open Sans', sans-serif;
  }
  input, select, textarea {
    border: 1px solid #cdd1d4;
  }
  input:focus,
  select:focus,
  textarea:focus {
      box-shadow: 0 0 0 2px #b2cfe7;
      transition: 0.25s;
  }
  .visually-hidden:not(:focus):not(:active),
  input[type="checkbox"].visually-hidden,
  input[type="radio"].visually-hidden {
    position: absolute;
    width: 1px;
    height: 1px;
    margin: -1px;
    border: 0;
    padding: 0;
    clip-path: inset(100%);
    clip: rect(0 0 0 0);
    overflow: hidden;
  }
  .container {
    max-width: 705px;
    padding: 0 20px;
    margin: 50px auto;
  }
   .form__box {
    background-color: #fff;
    padding: 35px 45px;
    margin-bottom: 50px;
    border: 4px solid #f0f0f0;
  }
  .form__group {
    background-color: #ffffff;
    padding: 10px 25px 25px;
  }
  .form__title {
    font-size: 24px;
    font-weight: 700;
    margin-bottom: 20px;
    position: relative;
    display: flex;
    align-items: center;
    padding: 0 25px 20px;
    border-bottom: 1px solid #cdd1d4;
    &::before {
      content: '';
      width: 40px;
      height: 40px;;
      display: block;
      margin-right: 15px;
      background-repeat: no-repeat;
      background-position: center center;
    }
    

  }
  .form__field {
    display: flex;
    align-items: center;
    margin-bottom: 15px;
    font-size: 15px;
    &_notification {
      max-width: 310px;
      margin-left: auto;
      margin-bottom: 10px;
      margin-top: 25px;
      input {
        width: 20px;
        height: 20px;
        margin-right: 10px;
      }
    }
    & span {
      color: #df0024;
    }
    &:last-child {
      margin-bottom: 0;
    }
  }
  .form__label {
      flex: 0 1 217px;
      color: #6a6a6a;
      margin-right: 30px;
      cursor: default;
      font-weight: 700;
      position: relative;
      &_required {
        &::after {
          position: relative;
          content: '*';
          color: #df0024;
          left: 3px;
        }
      }
  }
  .form__field-wrap {
    flex: 0 1 310px;
    & input {
      font: inherit;
      padding: 10px 15px 10px 15px;
      width: 100%;
      box-sizing: border-box;
    }
     &_date {
      flex: none;
    }
    &_gendere {
      & div {
        display: flex;
        align-items: center;
        margin-bottom: 10px;
      }
      & input {
        width: 20px;
        height: 20px;
        margin-right: 10px;
      }
    }
    &_house {
      flex: 0 1 150px;
    }
    &_select {
      & .is-invalid {
        background-image: none;
      }
    }
  }
  .form__select {
    font: inherit;
    width: 100%;
    box-sizing: border-box;
    & option {
      padding: 5px 35px 5px 15px;
    }
    &_doctors {
      padding: 10px 35px 10px 15px;
    }
  }
  .form__textarea {
    font: inherit;
    resize: none;
    padding: 10px 15px 10px 15px;
    width: 100%;
    box-sizing: border-box;
    height: 85px;
    line-height: 1.4;
  }
  .form__errors {
    font-size: 12px;
    margin-top: 5px;
  }
  .form__buttons {
    display: flex;
    justify-content: space-between;
  }
  .button {
    font: inherit;
    font-weight: 600;
    height: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #ffffff;
    background-color: #005faf;
    border: 1px solid #005faf;
    padding: 0 25px;
    border-radius: 3px;
    transition: 0.25s;
    &_reset {
      color: #005faf;
      background-color: #ffffff;
    }
    &:hover {
      opacity: 0.8;
    }
    &:focus {
      outline: 2px solid #cdd1d4;;
    }
  }
   .invalid-feedback {
    color: #df0024;
  }
  .is-invalid {
    border: 1px solid #df0024;
    padding-right: calc(1.5em + .75rem);
    background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' fill='none' stroke='%23dc3545' viewBox='0 0 12 12'%3e%3ccircle cx='6' cy='6' r='4.5'/%3e%3cpath stroke-linejoin='round' d='M5.8 3.6h.4L6 6.5z'/%3e%3ccircle cx='6' cy='8.2' r='.6' fill='%23dc3545' stroke='none'/%3e%3c/svg%3e");
    background-repeat: no-repeat;
    background-position: right calc(.375em + .1875rem) center;
    background-size: calc(.75em + .375rem) calc(.75em + .375rem);
  }
  .no-icon {
    background-image: none;
  }
  @media (max-width: 768px) {
    .container {
      max-width: 550px;
      margin: 20px auto;
    }
    .form__field {
      flex-direction: column;
      align-items: inherit;
      font-size: 13px;
      &_notification {
        flex-direction: row;
        align-items: center;
        max-width: inherit;
      }
    }
    .form__field-wrap {
      flex: 0;
    }
    .form__label {
      flex: 0;
      margin-right: 0;
      margin-bottom: 10px;
    }
    .form__title {
      font-size: 20px;
    }
    .button {
      font-size: 14px;
    }
  }
  @media (max-width: 480px) {
    .form__box {
      padding: 25px 15px;
    }
    .form__buttons {
      flex-direction: column;
    }
    .form__button {
      &_register {
        order: -1;
        margin-bottom: 20px;
      }
    }
    .form__group {
      padding: 0;
    }
    .form__title {
      padding: 0 0 20px;
    }
  }
</style>