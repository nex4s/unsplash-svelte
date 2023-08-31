<!-- Todo.svelte -->
<script>
  // @ts-nocheck
  
  
    let client_id = 'PQcfzFaoU4aWTpPwGTE25RcTCqeDcUNbgbYhBy_H3Ww';
  let query = ''
    function getImage(query, page) {
      const requestUrl = `https://api.unsplash.com/search/photos?query=${query}&client_id=${client_id}&page=${!page ? 1 : parseInt(page)}`;
  
      return fetch(requestUrl)
        .then(response => response.json())
        .then(data => {
          const regularImageUrls = data.results.map(image => image.urls.regular);
          return regularImageUrls;
        })
        .catch(error => {
          console.error('Error fetching data:', error);
          return [];
        });
    }
  
    let images = []; // Initialize images array
    let page = 1
  
  
    let currentIndex = 0;
  
    function updateImages() {
      getImage(query, page).then(regularImageUrls => {
        images = regularImageUrls;
        currentIndex = 0; // Reset currentIndex when images change
      });
    }
    
    function next() {
      currentIndex = (currentIndex + 1) % images.length;
    }
  
    function previous() {
      currentIndex = (currentIndex - 1 + images.length) % images.length;
    }
  
    function handleKeyPress(event) {
    if (event.key === "Enter") {
      event.preventDefault(); // Prevent the default form submission behavior
      updateImages(); // Call the function to update images based on the entered query
    }
  }
  </script>
  
  
  <html class="h-full bg-black" lang="en">
  <div class="gallery">
    <img class="absolute left-1/2 top-1/2 transform -translate-x-1/2 -translate-y-1/2 border-solid rounded-lg border-gray-400 border-opacity-50" width="350px" src={images[currentIndex]} alt="Gallery Image">
    <div class="arrows absolute flex justify-between w-full top-1/2 transform -translate-y-1/2">
      <button class="ml relative bg-transparent text-white font-sans font-bold text-7xl" on:click={previous}>
        <div class="crcl"></div>⮘</button>
      <button class="mr relative bg-transparent text-white font-sans font-bold text-7xl" on:click={next}>
        <div class="crcl"></div>⮚</button>
    </div>
  
    <div class="input-container">
      <input
        class="input-field"
        bind:value="{query}"
        on:keydown={handleKeyPress}
        placeholder="Enter query..."
      />
      <input
        class="input-field"
        type="number"
        bind:value="{page}"
        on:keydown={handleKeyPress}
        placeholder="Enter page..."
      />
    </div>
  </div>
  </html>
  
  
  
      <style>
        
  .crcl {
    font-family: galano;
  }
  
        .mr {
          margin-right: 250px
        }
  
  
        .ml {
          margin-left: 250px;
        }
  
        @media (min-width: 769px) {
    .input-container {
      justify-content: center;
      display: flex;
      align-items: center;
      font-family: 'Courier New', Courier, monospace;
      font-weight: 800;
      border-color: transparent;
      border-style: solid;
      border-radius: 20px;
      height: 30px;
    }
  
    .input-field {
      margin-left: 10px;
      margin-right: 10px;
    }
  }
  
  @media (max-width: 768px) {
    .gallery {
      display: flex;
      flex-direction: column;
      align-items: center;
    }
  
    .input-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 20px;
    }
  
    .input-field {
      width: 80%;
      max-width: 300px;
      margin-bottom: 10px;
    }
  }

  
  @media (max-width: 768px) {
  .arrows {
    display: flex;
    justify-content: space-between;
    width: 100%;
    max-width: 300px;
    margin-left: auto;
    margin-right: auto;
    position: fixed;
    bottom: 20px;
    left: 0;
    right: 0;
    padding: 0 10px;
z-index: 1;
  }

  .ml,
  .mr {
    margin-left: 0;
    margin-right: 0;
  }
}

        
      </style>
  
