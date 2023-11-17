<template>
    <div>

        <!--Home Container-->
        <div style = "margin-top:10%; width:100%; display:flex; flex-direction: column; gap:50px; align-items: center;">

        <!--Title Container-->
        <div style = "margin-right:100px; display:flex; flex-direction: row; gap:30px;">
            <img src= "/assets/img/Google_2015_logo.svg.png" width = "200">
            <h style = "color: #666; font-size: 48px; margin-top: 0.25rem;">Book Finder</h>
        </div>

        <!--Search Form and Search Button Container-->
        <div style = "display:flex; flex-direction: row; gap:50px;">
            <input v-model = "searchForm" id = "search_form" style = "padding-left:20px; width:500px; height:50px; border: 1px solid #B0B0B0; background: #FFF; border-radius: 50px; z-index: 999;" type="text">
            <button @click="searchBooks" id = "search_btn" style = "border:none; color:#666; padding:20px 25px 20px 25px; font-size: medium; border-radius: 10px; cursor:pointer;">Search</button>
        </div>

        <!--Container for the books-->
        <div style = "display:flex; flex-direction: row; flex-wrap: wrap; gap:20px; margin:20px;">
          <div v-if="!searchResults" style = "display:flex; justify-content: center;">
            <p style = "font-size: xx-large; color:#666;">No Results</p>
          </div>
            <!--Book-->
            <img
                v-for="result in searchResults"
                :key="result.id"
                :src="result.volumeInfo.imageLinks.thumbnail"
                :alt="result.volumeInfo.title"
                style="background-color: #FF8383; height: 300px; width: 200px; cursor:pointer;"
                @click="imageID(result)"
            />

        </div>

        </div>

        <BookModal :modalActive="modalActive" @close-modal="handleCloseModal">
        <!-- Main container -->
          <div style="display:flex; flex-direction: row; justify-content: center; gap:50px;">
            <img :src = "modalBookData.image" style = "width:200px; height:auto;">
            <div style="display:flex; flex-direction: column; justify-content: center;">
              <p style = "font-size: xx-large; color: #666;">{{ modalBookData.title }}</p>
              <p style = "font-size: large; color: #666;">{{ modalBookData.authors }}</p>
              <p style = "font-size: large; color: #666;">{{ modalBookData.description }}</p>
            </div>
          </div>
      </BookModal>



    </div>


</template>

<script setup>
import BookModal from './BookModal.vue';


import { ref } from 'vue';
import axios from 'axios';

const apiUrl = 'https://www.googleapis.com/books/v1/volumes';
const apiKey = 'AIzaSyBGt5zZ1YUenUdt-oKPELshJSPruwFGBII'; 


const searchForm = ref("");
// Search results
const searchResults = ref([]);


const searchBooks = async () => {
  try {
    // Make the API request using Axios
    const response = await axios.get(apiUrl + "?q=" + searchForm.value + "&key=" + apiKey);

    // Update the searchResults variable
    searchResults.value = response.data.items;
  } catch (error) {
    console.error('Error fetching data from Google Books API:', error.response.status, error.response.data);
  }
};

//toggle modal and get ID of the image and transfer data to modal
const modalBookData = ref(null);

const imageID = (image_id) => {
  toggleModal();
  modalBookData.value = {
    image: image_id.volumeInfo.imageLinks.thumbnail,
    title: image_id.volumeInfo.title,
    authors: image_id.volumeInfo.authors,
    description: image_id.volumeInfo.description,
  };


};

const modalActive = ref(null);
const toggleModal = () => {
    modalActive.value = !modalActive.value;
};

const handleCloseModal = () => {
  toggleModal();
  modalBookData.value = null;
};



</script>

<style>
  @import './assets/styles/style.css';

  #search_btn:hover{
    box-shadow: 0px 0px 3px 0px rgba(0, 0, 0, 0.25);
    transform: scale(1.05, 1.05);
  }
  #search_btn:active{
    box-shadow: 0px 0px 3px 0px rgba(0, 0, 0, 0.25);
    transform: scale(0.95, 0.95);
  }
  #search_form:hover{
    box-shadow: 0px 0px 3px 0px rgba(0, 0, 0, 0.25);
    transform: scale(1.05, 1.05);
  }

  #bookshelf_btn:hover{
    box-shadow: 0px 0px 3px 0px rgba(0, 0, 0, 0.25);
    transform: scale(1.05, 1.05);
  }
  #bookshelf_btn:active{
    box-shadow: 0px 0px 3px 0px rgba(0, 0, 0, 0.25);
    transform: scale(0.95, 0.95);
  }
</style>