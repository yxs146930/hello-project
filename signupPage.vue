
<template>
    <form @submit.prevent="validateBeforeSubmit" class="row">
         <div class="col-md-3">
         </div>
      <div class="col-md-6">
        <b-card>
          <div slot="header">
            <strong>Nativee Registration Form</strong> 
          </div>
          <b-form-fieldset
            description="Let us know your full name."
            label="Enter your name"
            :label-cols="3"
            :horizontal="true">
            <b-form-input v-model="signupModel.name" type="text"></b-form-input>
          </b-form-fieldset>
          <b-form-fieldset
            description="The customer will see your nickname"
            label="Enter nickname"
            :label-cols="3"
            :horizontal="true">
            <b-form-input v-model="signupModel.nickName" type="text" placeholder="Nickname"></b-form-input>
          </b-form-fieldset>
          <b-form-fieldset
            description="Please enter your email"
            label="Email Input"
            :label-cols="3"
            :horizontal="true">
            <b-form-input v-model="signupModel.email" type="email" placeholder="Enter your email"></b-form-input>
          </b-form-fieldset>
          <b-form-fieldset
            description="Please enter a complex password"
            label="Password Input"
            :label-cols="3"
            :horizontal="true">
            <b-form-input v-model="signupModel.password" type="password" placeholder="Enter your password"></b-form-input>
          </b-form-fieldset>
          <b-form-fieldset
            description="Please repeat your password"
            label="Password Again"
            :label-cols="3"
            :horizontal="true">
            <b-form-input v-model="repassword" type="password" placeholder="Repeat your password"></b-form-input>
          </b-form-fieldset>
          <b-form-fieldset
            label="Select"
            :label-cols="3"
            :horizontal="true">
            <b-form-select v-model="signupModel.gender"
              :plain="true"
              :options="['Male', 'Female', 'NotToSpecific']"
              value="Please select">
            </b-form-select>
          </b-form-fieldset>
        <b-form-fieldset
            description="Please provide your invitation code"
            label="Invitation Code"
            :label-cols="3"
            :horizontal="true">
            <b-form-input type="text" placeholder="Invitation Code"></b-form-input>
          </b-form-fieldset>
          <div slot="footer">
            <b-button type="submit" size="sm" variant="primary"><i class="fa fa-dot-circle-o"></i> Submit</b-button>
            <b-button v-on:click="clear()" type="reset" size="sm" variant="danger"><i class="fa fa-ban"></i> Reset</b-button>
          </div>
          <div v-if="failed"> <h5>{{ promptInfo }} </h5></div>
        </b-card>
      </div>
    </form>
</template>
<script>
  import Constant from '@/constant/Constant'
  export default {
    data() {
      return {
        failed: false,
        promptInfo: '',
        repassword: '',
        name: '',
        signupModel: {
            name: '',
            nickName: '',
            email: '',
            password: '',
            gender: '',
            invitationCode: ''
        }
      }
    },
    created() {
        this.signupModel.gender = 'Female'
    },
    methods: {
        validateBeforeSubmit: function() {
            this.$validator.validateAll().then((result) => {
            if (result) {
                this.signup = false
                this.save();
                return;
            }
            this.failed = true
            })
        },
        signup: function() {
            const apiUrl = Constant.SERVER_BASE_URL + Constant.post_nativee_signup
            const paramData = this.signupModel;
            this.$http.post(apiUrl, paramData).then(response => {
                console.log(response)
                if (response.data.operationResponse.result == 'UNSUCCESSFUL') {
                    this.failed = true;
                    this.promptInfo = response.data.operationResponse.reason;
                    console.log(this.promptInfo);
                }
            })
        },
        clear: function() {
            // console.log(this.signupModel.gender)
            this.signupModel.name = ''
            this.signupModel.nickName = ''
            this.signupModel.email = ''
            this.signupModel.gender =''
            this.signupModel.invitationCode = ''
            this.repassword = ''
        }
    }
  }
</script>
<style scoped>
.header strong { 
    text-align:center;
}
.cis-danger {
  color: red;
}
.cis-info {
  color: blue;
}
</style>

