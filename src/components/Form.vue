<template class="bg-dark">

     <div class="bg-dark" >
         <!-- <link rel="stylesheet" type="text/css" href="style.css"> -->
         <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css">
                                      <h4 style="margin-left: 15px; font-family:'Poppins'; color: white;">Cart</h4>

        <div v-for='lesson in cart' :key="lesson.id" class="lesson_cards">
        <div class="card shadow p-3 mb-5 bg-dark rounded" style="width: 40rem; margin-left: 15px; font-family: Poppins; color: white; "  >
        <div style="display: flex; flex-direction:row; flex-wrap: wrap; justify-content: space-around; align-items: center;">

                       <img v-bind:src="lesson.image"  class="card-img-top ps-10" style="width:15%; ">
                        <p class="card-text" >Name: {{lesson.subjectname}}</p>
                        <p class="card-text" >Location: {{lesson.location}}</p>                 
                        <p class="card-text" >Price:{{lesson.price}}</p>

                    <!-- Adding a "Remove" button to remove the lesson from cart -->
                    <button class="btn btn-danger" style="margin-top: 1px;  margin-bottom: 15px;" @click ="remove(lesson)" >
                       Remove
                    </button>
                <!-- End of lesson card -->
                
                </div>
                </div>
        </div>
    <!-- Checkout Form -->
        <div class="checkoutForm">
                                        <div style="display: flex; flex-direction:column;">              
                        <h4 style="margin-left: 15px; font-family:'Poppins' ; color: white;">Checkout</h4>
                    <div class="card shadow p-3 mb-5 bg-dark rounded" style="width: 40rem; margin-left: 15px;color: white; font-family: Poppins;"  >
            <strong>{{successfulCheckout}}</strong>
            <!-- Name input field -->
             <div class="mb-3">
             <label for="name"  class="form-label">Name</label>
                <input type="text" v-model="name"  class="form-control text-light" style="background: none;"  placeholder="Enter Name..." v-on:keypress="isLetter($event)">
             </div>
            
            <!-- Phone number field -->
             <div class="mb-3">
             <label for="phoneno"  class="form-label">Phone Number</label>
                <input type="text" v-model="phone"  class="form-control text-light" style="background: none;" placeholder="0123" v-on:keypress="isNumber($event)"> <p></p>
             </div>            
            <!-- Checkout button. Only enabled if values to Name and Phone are added -->
            <button class="btn btn-primary" v-if="name == '' || phone == ''" disabled = "disabled">Checkout</button>
            <button class="btn btn-primary" v-else @click="CheckoutLessons()">Checkout</button>
            <!-- <button class="btn btn-primary" v-on:click="CheckoutLessons()" type="submit" :class="{ disabled: isDisabled }" :disabled="isDisabled">Submit</button>               -->

            <p style="font-size: 1.5rem; color: red; margin-top: 0; padding-bottom: 70px; padding-left: 10%;">{{errorMessage}}</p>
            
        </div>
  </div>
  </div>
  </div>
</template>

<script>
import AppVue from '@/App.vue';
export default {
  name: "FormCheckoutPage",
  props: ['cart'],
 AppVue,
  data() {
    return {
      name: "",
      phone: "",
      errorMessage: "",
      successfulCheckout: "",
      
    };
  },
  methods: {
      isLetter(e){
            //for every character input
            let char = String.fromCharCode(e.keyCode); 

            //if the character is an alphabet
            if(/^[A-Za-z]+$/.test(char)){
                this.errorMessage = "";
                return true;
            }  
            else{
                //displaying error message
                this.errorMessage = "Please enter only characters";
            } 
            
    },
    //function to check if the user input for phone has only numbers
        isNumber(e){

            //matching the user input with the character code of 0-9
            if(e.charCode >= 48 && e.charCode <= 57 ){
                this.errorMessage = "";
                return true;
            }
            else{
                //displaying error message
                this.errorMessage = "Please enter only numbers";
            }
        },
        CheckoutLessons(){
                    if(this.errorMessage == "" && this.cart.length > 0){
                        alert("Successfully placed order!");
                        this.successfulCheckout = "Thank you for purchasing!";
                        this.name = "";
                        this.phone = "";
                        this.cart.splice(0, this.cart.length);
                        
    
                    }
                    else if(this.cart.length == 0){
                        alert("No products in cart!")
                    }
                    else{
                        alert("Please enter the correct values.");
                    }
                    
                },
        remove(lesson){
            console.log("removing lesson from cart...");
            this.$emit('removeLesson', lesson);
        }
  }
  
};
</script>