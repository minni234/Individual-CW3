
<template  class="bg-dark">

    <div class="bg-light" style="padding: 15px; display:flex; flex-direction:row; justify-content: space-evenly; flex-wrap: wrap; ">
        <!-- <link rel="stylesheet" type="text/css" href="style.css"> -->
        <div  v-for="lesson in lessons" :key="lesson.id" class="lesson_cards">

            <!-- Binding an image -->
            
                        <div  class="card shadow p-3 mb-5 bg-light rounded" style="width: 18rem; font-family: Poppins; color: black;" >

            <!-- Displaying the details -->
              <div style="width: 100%; height: 100%;">
                <img v-bind:src="lesson.image"  class="card-img-top ps-10" style="width:40%; object-fit: cover; ">
            </div>
                      <p class="card-text">Name: {{lesson.subjectname}}</p>
                        <p class="card-text">Location: {{lesson.location}}</p>                 
                        <p class="card-text">Price:{{lesson.price}}</p>
                <p class="card-text">Number of Spaces : {{lesson.numberofspaces }}</p>


         

            <!-- Displaying the "Add to Cart" Button -->
             <button v-on:click='addLessonToCart(lesson)'  v-if='canAddToCart(lesson)'  class="btn btn-primary " style="margin-top: 20px; margin-bottom: 15px;">Add to cart
                    <i class="fas fa-shopping-bag"></i>
                </button>
                <button disabled="disabled" class="btn btn-primary " style="margin-top: 20px; margin-bottom: 15px;" v-else>Add to cart</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: "ProductList",
  props: ['lessons'],
  data() {
      
    return {
        //fetching the lessons from server
    // lessons: [],
    };
  },
//   mounted(){            //created function fetches the lessons

//     //fetching the lessons from server
//     fetch('https://cst3145cwhadassah.herokuapp.com/collection/lessons').then( response => {
//         response.json().then(json => {
//             this.lessons = json;
//             console.log(json);
//         } )

//     });
        
//     },
  methods: {
    addLessonToCart(lesson) {
      console.log("added product", lesson.id);
      this.$emit('addLesson',lesson);
    },
    canAddToCart(lesson){
        return lesson.numberofspaces > 0;
    },
    fetchlessons(){
        //fetching the lessons from server
fetch("https://cst-individual.herokuapp.com/collection/lesson_information")
.then(
        function(response){
            response.json().then(
                function(json){
                    return json;
                });
        });
    }
  },
};
</script>