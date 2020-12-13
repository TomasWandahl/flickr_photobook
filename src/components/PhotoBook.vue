<template>
  <div class="PhotoBook">
    <form class="search-form" @submit.prevent="fetchPhotos()">
      <input class="search-bar" type="text" v-model="searchQuery" placeholder="Search for a photo tag....">
      <button type="submit">Sök</button>
    </form>
    
    <ImageCarousel>
      <Loader v-show="loading" />
        <CarouselImage v-for="(image, index) in images" 
        :key="image" 
        :index="index"
        :visibleImages = "visibleImages"
        >
            <img :src="image.url" />
        </CarouselImage>
    </ImageCarousel>
  </div>

  
</template>

<script>
const axios = require('axios');
import ImageCarousel from './ImageCarousel'
import CarouselImage from './CarouselImage'
import Loader from './Loader'

export default {
  name: 'PhotoBook',
  data() {
    return {
      searchQuery: '',
      images: {},
      visibleImages : [],
      timeout : null,
      loading: false,
    }
  },
  components : {
    ImageCarousel, CarouselImage, Loader
  },
  beforeMount() {

  },
  methods: {
    initFetch() {
     
    },
    async fetchPhotos() {
      // reset carousel
      this.images = {};
      this.visibleImages= [];
      clearTimeout(this.timeout);
      this.loading = true;

      
      
      const {data: response} = await axios.get(`https://cors-anywhere.herokuapp.com/flickr.com/services/feeds/photos_public.gne?tags=${this.searchQuery}&format=json&nojsoncallback=1`);
      let id = 0;
      this.images = response.items.map(item => {
        return {url : item.media.m, id : id++};
      });

      this.loading = false;
      this.displayAndSwapImages();
      
    },
    displayAndSwapImages(i = 0) {  
      this.visibleImages = [i];
      i = (i >= this.images.length - 1) ? 0 : i + 1;
      this.timeout = setTimeout(() => {
        this.displayAndSwapImages(i)
      }, 3000);
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

  .PhotoBook {
    width: 70%;
    margin: 0 auto;
  }
  .carousel {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    width: 55vw;
    overflow: hidden;
    margin: 0 auto;
    height: 500px;
    position: relative;
  }
  img {
    width: 55vw;
    object-fit: cover;
    box-shadow: 1px 3px rgba(0,0,0, 0.30);
  }
  
  .search-form {
        display:flex;
        justify-content: center;
        flex-wrap: wrap;
        width: 44%;
        margin: 0 auto;
        
    }

    .search-form button {
        height: 50px;
        padding: 5px;
        width: 125px;
        margin: 20px 10px 20px 10px;
        border: none;
        border-radius: 4px;
        box-shadow: 1px 3px rgba(0,0,0, 0.30);
        font-size: 15px;
        font-weight: bold;
        font-family: 'montserrat', sans-serif;
        color: #3b5166;
        cursor: pointer;
    }

    .search-bar {
        width: 100%;
        height: 50px;
        padding: 20px;
        border: none;
        appearance: none;
        font-size: 20px;
        box-shadow: 1px 3px rgba(0,0,0, 0.30);
        outline: none;
        
    }
    .search-bar:focus, button:focus {
      outline: none;
    }
    @media screen and (max-width: 800px) {
        .search-form {
            width: 90vw;
        } 
        .carousel {
          width: 95vw;
        }
        img {
          width: 95vw;
        }
        .PhotoBook {
          width: 95vw;
        }
    }
    @media screen and (max-width: 340px) {
        .search-form {
            width: 90vw;
            
        } 
        .carousel {
          width: 95vw;
          height: 200px;
        }
        img {
          width: 95vw;
        }
        .PhotoBook {
          width: 95vw;
        }
    }
</style>
