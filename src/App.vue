<script setup>
import { ref } from 'vue';

const showModal = ref(false);
const cardName = ref("");
const desc = ref("");
const image = ref(null);
const notes = ref([]);

const addNote = () => {
  notes.value.push({
    id: Math.floor(Math.random() * 100000),
    cardName: cardName.value,
    desc: desc.value,
    image: image.value,
    color: getRandomColor(),
    date: new Date()
  });
  cleanForm();
  showModal.value=false;
};

const createObjectURL = (image, index) => {
  if (!image) {
    console.error(`Image not found for note at index ${index}`, notes[index]);
    return '';
  }
  return URL.createObjectURL(image);
};

const cleanForm = () => {
  cardName.value = "";
  desc.value = "";
  image.value = null;
};

const handleFileUpload = (event) => {
  image.value = event.target.files[0];
};

function getRandomColor(){
  const color="hsl("+Math.random()*360+",100%,75%)";
  return color;
}
const  formattedDate=(dateString)=> {
      const date = new Date(dateString);
      const day = date.getDate();
      const month = date.getMonth() + 1; // Month is zero-based
      const year = date.getFullYear();
      
      // Pad single digits with leading zero
      const formattedDay = String(day).padStart(2, '0');
      const formattedMonth = String(month).padStart(2, '0');
      
      return `${formattedDay}/${formattedMonth}/${year}`;
    }

</script>

<template>
  <div>
    <nav class="navbar">
      <div class="navdiv">
        <div class="logo"><a href="#">MinNote</a></div>
        <ul>
          <li><a href="#">Home</a></li>
          <li><a href="#">About</a></li>
          <li><a href="#">Contact</a></li>
          <button @click="showModal = true">+ Notes</button>
        </ul>
      </div>
    </nav>
    
    <div v-if="showModal" class="overlay">
      <div class="form-div">
        <label for="image">Image:</label>
        <img id="priview-img" class="priview-img" :src="image ? createObjectURL(image,0) : ''" alt="Uploaded Image" v-if="image">
        <input id="image" name="image" type="file" @change="handleFileUpload" ref="fileInput">

        
        
        <label for="cardName">Card Name:</label>
        <input type="text" v-model="cardName" placeholder="Enter Card Name" required>
        
        <label for="desc">Description:</label>
        <textarea v-model="desc" rows="5" placeholder="Enter your description" required></textarea>
        
        <button class="form-Btn-Submit" @click="addNote">Submit</button>
        <button class="form-btn-close" @click="showModal = false">Close</button>
      </div>
    </div>
    
    <div class="card-container">
      <div v-for="(note,index) in notes" :key="note.id" class="card">
        <img :src="createObjectURL(note.image, index)">
        <div class="card-content">
          <h3>{{ note.cardName }}</h3>
          <p>{{ note.desc }}</p>
          <p :style="{backgroundColor: note.color}">{{ formattedDate(note.date) }}</p>
          <a href="" class="btn">Read More</a>
        </div>
      </div>
    </div>
  </div>
</template>




<style scoped>
.overlay {
  display: true;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); 
  z-index: 999; 
}



.addNote:hover .overlay{
opacity: 0.8;
}

.navbar{
  padding: 20px;
  background: crimson; 
  font-family: calibri;
}
.navdiv{
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.logo a{
  font-size: 35px; font-weight: 600; color: white; text-decoration: none;
}
li{
  list-style: none; display: inline-block;

}
li a{
  color: white; font-size: 18px; font-weight: bold; margin-right: 25px; text-decoration: none;
}
button{
  color: white; background-color: black; margin-left: 10px; border-radius: 10px; padding: 10px; width: 90px;
}
.form-btn-close{
  color: white; background-color: red; margin-left: 10px; border-radius: 10px; padding: 10px; width: 90px;
}
.form-btn-close{
  color: white; background-color: red; margin-left: 10px; border-radius: 10px; padding: 10px; width: 90px;
}
button a{
  color: white; font-weight: bold; font-size: 15px; text-decoration: none;
}
/* this is for card view */
*{
  margin:0px;
  padding:0px;
  box-sizing: border-box;
  font-family: sans-serif;
}
body{
  background-color: #f0f0f0;
}
.card-container{
  display:flex;
  justify-content: center;
  flex-wrap:wrap;
  margin-top:100px;
}
.card{
  width:325px;
  background-color: #f0f0f0;
  border-radius: 8px;
  overflow:hidden;
  box-shadow: 0px 2px 4px rgba(0,0,0,0.2);
  margin:20px;

}
.priview-img {
  width: 200px; 
  height: 130px; 
  border-radius: 5%; 
  padding:16px;
}

.card img{
  width: 100%;
  height:auto;
}
.card-content{
  padding:16px;
}
.card-content h3{
  font-size:28px;
  margin-bottom: 8px;
}
.card-content p{
  color:#666;
  font-size: 15px;
  line-height: 1.3;
}
.card-content .btn{
  display: inline-block;
  padding: 8px 16px;
  background-color: #333;
  text-decoration: none;
  border-radius: 4px;
  margin-top: 16px;
  color: #fff;
}
/* html form css */
.form-div {
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    width: 300px;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%); 
  }

  label {
    font-weight: bold;
    margin-bottom: 8px;
    display: block;
  }

  input[type="text"],
  textarea {
    width: 100%;
    padding: 8px;
    margin-bottom: 16px;
    border: 1px solid #ccc;
    border-radius: 4px;
    resize: none; /* Prevent text area resizing */
  }

  input[type="submit"] {
    background-color: #007bff;
    color: #fff;
    border: none;
    padding: 10px 16px;
    border-radius: 4px;
    cursor: pointer;
  }

  input[type="submit"]:hover {
    background-color: #0056b3;
  }

</style>