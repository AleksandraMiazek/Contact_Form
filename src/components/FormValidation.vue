<template>
<div>

<div class="col-lg-offset-4 col-md-6 col-lg-5 container">
			<div class="panel panel-primary" id="formdiv">
				<div class="panel-heading">
					<h1 class="panel-title">Contact Form</h1>
                    <h6>Please fill in all form elements</h6>
				</div>
				<div class="panel-body">
					<form @submit.prevent=''>
						<div class="form-group">
							<label for="exampleInputEmail1"><b>Name</b></label>
							<input type="text" placeholder="Enter your name" name="name" class="form-control" 
                                    minlength="5" maxlength="50" required v-model='name'>
                            <span class="alert" v-if='msg.name'> {{msg.name}} </span>
						</div>
						<div class="form-group">
							<label for="exampleInputEmail1"><b>Email</b></label>
							<input type="text" placeholder="Enter your email adress" name="email" class="form-control" required v-model='email'>
                            <span v-if='msg.email' class="alert"> {{msg.email}} </span>
						</div>			
						<div class="form-group">
							<label for="exampleInputEmail1"><b>Subject</b></label>
							<input type="text" placeholder="Subject" name="subject" class="form-control" maxlength="100" optional v-model='subject'>
                            <span class="alert" v-if='msg.subject'> {{msg.subject}} </span>
                        </div>
                        <div class="form-group">
							<label for="exampleInputEmail1"><b>Message</b></label>
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
      if(
          this.msg['name'] =='' &&
          this.msg['email'] == ''
          ) {
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
      } else {
          alert('Please enter the correct details');
      }
    }

    }
}
</script>

<style scoped>

.container {
    background-color: #ffffff;
	padding: 35px;
	margin-left: auto;
	margin-right: auto;
	margin-top: 60px;
	-webkit-box-shadow: 3px 3px 30px 5px rgba(204,204,204,0.9);
	-moz-box-shadow: 3px 3px 30px 5px rgba(204,204,204,0.9);
	box-shadow: 3px 3px 30px 5px rgba(204,204,204,0.9);
}
h1 {
    font-family: 'Lobster', cursive;
    font-size: 46px;
}
h6 {
    color: rgb(124, 124, 124);

}
.alert {
    color: rgb(201, 24, 24);
    padding: 0;
}
input[type=text],
textarea
{
	background-color: #f8f7f7;
	color: #666;
	border: 1px solid #ddd;
	border-radius: 5px;
	box-sizing: border-box;
	outline: none;
}

input[type=text]:focus,
textarea:focus
{
	-webkit-box-shadow: 0px 0px 5px 2px rgba(204,204,204,0.9);
	-moz-box-shadow: 0px 0px 5px 2px rgba(204,204,204,0.9);
	box-shadow: 0px 0px 5px 2px rgba(204,204,204,0.9);
    border: 2px solid #8daaca;
    background-color: rgb(241, 247, 255);
}

input[type=submit]
{
	background-color: #3683b0;
	color: white;
	padding: 10px 15px;
	border: none;
	border-radius: 5px;
	cursor: pointer;
	letter-spacing: 1px;
}


</style>