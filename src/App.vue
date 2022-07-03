<template class="bg-dark">


  <div id="app" class="bg-light">
    <link rel = "shortcut icon" href ="#">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css">
    <!-- <link rel="stylesheet" type="text/css" href="style.css"> -->
   
  
    
  
    
            <!-- <a href="#" class="logo"><i class="fas fa-school" style="font-size: 25px;background: -webkit-linear-gradient(#f3b3c6, #f7e0a2);-webkit-background-clip: text;-webkit-text-fill-color: transparent;">&nbsp; ASC</i></a> -->
            
            <!-- creating a navigation bar -->
            <nav class="navbar navbar-expand-lg navbar-light bg-light shadow p-3 mb-3 bg-light ">
            <div class="container-fluid">
             <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <h3 style="color: #6da7ff; font-family: Poppins; padding-left: 15px; font-weight: bold;">{{sitename}}</h3>

            </div>
            <input v-on:input="fetchFilteredLessons" class="form-control w-auto  text-light" type="text" style="background: none;"  id="searchinput" v-model="inputSearch" placeholder="Search" />

            <h6 style="font-family: Poppins; color: black; margin-top: 5px; margin-right: 10px;" >Sort:</h6>
          <select v-on:change="reorderList" v-model="items_order"  class="form-select w-auto  text-dark bg-light" aria-label="Default select example">
              <option value="">-- Sort order --</option>
              <option value="ascending">Ascending</option>
              <option value="descending">Descending</option>
          </select>
          
            <div  @click="showCheckout" v-if="cartItemCount >= 1"  class="d-flex align-items-center p-2">
               <i class="fas fa-cart-plus" style="color: white; "></i>
                <span class="badge rounded-pill badge-notification bg-danger" >{{cartItemCount}}</span>
            </div>

          
            <div class="checkout"  v-else>
               <i class="fas fa-cart-plus" style="color: grey;"></i>
            <span class="badge rounded-pill badge-notification bg-danger"></span>

            </div>
            </div>
            </nav>

          
    
        <!-- <component :is="currentView"/> -->
        <div v-if="showLessons" class="bg-light" >
           

              
                <section class="course"  id="courses">
                 

                <main>
                    <lesson-list :lessons="lessons" @addLesson="addLessonToCart"></lesson-list>
                </main>
                </section>
            </div>
            <div  class="checkoutPage" v-else  style="color: black; flex-direction: column; justify-content: center; display: flex; align-items: center; font-family: Poppins; " >

               

                <!-- Displaying the lessons in the cart, if the cart is not empty -->
                <div  v-if ="this.cart.length > 0"  >
                    <checkout :cart="cart" @removeLesson="removeLessonFromCart"></checkout>
                </div>
                <!-- If all products are removed from cart, button is displayed to take back to the products page -->
                <div class="bg-dark" v-else style="color: white; flex-direction: column; align-items: center; display: flex; margin-top: 30px; height:530px;  font-family: Poppins;">
                      Cart is Empty
                    <button @click ="showCheckout()" class="btn btn-primary">
                     You have no Lessons in your Cart. Continue Shopping
                    </button>
                </div>
        
        </div>
        
  </div>
 
</template>

<script>
import lessonList from "./components/LessonList.vue";
import checkout from "./components/Form.vue";

export default {
  name: "App",
  components: {
    lessonList,
    checkout
  },
  data() {
    return {
    sitename:"AfterSchool Club",
      cart: [],
      lessons :[],
      showLessons: true,
    };
  },
   mounted(){            //created function fetches the lessons

    //fetching the lessons from server
fetch("https://cst-individual.herokuapp.com/collection/lesson_information")
.then( response => {
        response.json().then(json => {
            this.lessons = json;
            console.log(json);
        } )

    });
        
    },
  methods: {
    // showCheckout() {},
    addLessonToCart(lesson) {
     console.log("addLesson event received by the root component.");
      //reduce the spaces by 1
      lesson.numberofspaces -= 1;
      this.cart.push(lesson);
    },
    showCheckout(){             
        this.showLessons = this.showLessons ? false : true;
    },
    removeLessonFromCart(lesson){
        //increase the spaces by one on the main page
        lesson.numberofspaces += 1;
        for(let i = 0; i <= this.cart.length; i++){
                if(this.cart[i].id === lesson.id){
                    this.cart.splice(i, 1);
                    break;
                }
        }
    }
    
  },
  computed: {
    //function to return cart length                
    cartItemCount(){
        return this.cart.length || '';
    }
  },
};

</script>