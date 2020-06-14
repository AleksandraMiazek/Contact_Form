<template>
<div>
  <h1>Form validation</h1>
<div class="col-lg-offset-4 col-lg-4">
			<div class="panel panel-primary" id="formdiv">
				<div class="panel-heading">
					<h3 class="panel-title">Contact Form</h3>
                    <h6>Please fill in all form elements</h6>
				</div>
				<div class="panel-body">
					<form @submit.prevent=''>
						<div class="form-group">
							<label for="exampleInputEmail1">Name</label>
							<input type="text" placeholder="Enter your name" name="name" class="form-control" 
                                    minlength="5" maxlength="50" required v-model='name'>
                            <span v-if='msg.name'> {{msg.name}} </span>
						</div>
						<div class="form-group">
							<label for="exampleInputEmail1">Email</label>
							<input type="text" placeholder="Enter your email adress" name="email" class="form-control" required v-model='email'>
                            <span v-if='msg.email'> {{msg.email}} </span>
						</div>			
						<div class="form-group">
							<label for="exampleInputEmail1">Subject</label>
							<input type="text" placeholder="Subject" name="subject" class="form-control" maxlength="101" optional v-model='subject'>
                            <span v-if='msg.subject'> {{msg.subject}} </span>
                        </div>
                        <div class="form-group">
							<label for="exampleInputEmail1">Message</label>
							<textarea rows="4" placeholder="Write your message..." name="message" class="form-control" maxlength="500" required
                            v-model='message'></textarea>
                            <span v-if='msg.message'> {{msg.message}} </span>
						</div>
						<input type="submit" value="Send" :disabled='disableSubmitButton' class="btn btn-warning" @click="add">
                       <!-- <button variant="primary" :disabled='disableSubmitButton' @click="add" >Send</button>-->
                    </form>
				</div>
			</div>
		</div>

</div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.min.css'
import axios from 'axios';
export default {
  name: 'FormValidation',
    data() {
        return {
            name:'',
            email:'',
            msg:[],
            subject:'',
            message:'',
            validationStatus:[],
        disableSubmitButton : true,
        }
    },

    watch:{
        name(value){
            this.name = value;
            this.check_name(value);
        },
        email(value){
            this.email = value;
            this.check_email(value);
        },
         subject(value){
            this.subject = value;
            this.check_subject(value);
        },
        message(value){
            this.message = value;
            this.check_message(value);
        }
    },

    methods: {
        check_name(value) {
            this.remainingChars = 5 - value.length;
            if (value.length < 5){
                this.msg['name'] = 'Name must contain min. 5 characters  ' + this.remainingChars + ' to go...';
                this.disableSubmitButton= true;
            } else if (value.length >= 5){
                this.msg['name'] = '';
                this.validationStatus['name'] = true;
            } else if (value.length > 50){
                this.msg['name'] = 'Name is too long! Allowed up to 50 characters';
                this.disableSubmitButton= true;
            }
            this.check_all();
        },

        check_email(value) {
            if (/^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(value))
            {
                this.msg['email'] = '';
                 this.validationStatus['email'] = true;
            } else {
                this.msg['email'] = 'Keep typing... We are waiting for correct email';
                this.disableSubmitButton= true;
            }
            this.check_all();
        },

        check_subject(value) {
            if (value.length > 100) {
                this.msg['subject'] = 'Subject is too long! Allowed up to 100 characters '
            }
        },

        check_message(value) {
            if (value.length > 500){
                this.msg['message'] = 'Message is too long! Allowed up to 100 characters '
                this.disableSubmitButton= true;
            }  else if (value.length < 500){
                this.validationStatus['message'] = true;
                this.msg['message'] = value.length + ' of the 500 allowed characters entered';
            this.check_all();
            } 
        },
       
        check_all() {
            if(
                this.validationStatus['name'] === true &&
                this.validationStatus['email'] == true &&
                this.validationStatus['message'] == true
            ) {
            this.disableSubmitButton= false;
            }
        },

        submit() {
            alert('Great! You have completed contact form correctly');
        },

       
    async add(){
      try {
       let response = await axios.post('https://5d9f7fe94d823c0014dd323d.mockapi.io/api/message', {
           name: this.name,
            email:  this.email,
            subject: this.subject,
            message: this.message
       });
       console.log('Sukces', response);
       alert ('You have completed contact form correctly. Upload successful');
      } catch (e) {
        alert('Upload failed, try again later! ');
        console.log('Error', e);
      }
    }

    }
}
</script>

<style scoped>

</style>