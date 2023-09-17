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
    <div class="fixed inset-0 flex items-center justify-center">
      <div class="border-transparent max-w-3xl mx-auto rounded-2xl border-2 border-solid p-6 backdrop-blur-sm bg-purple/40 text-center flex flex-col items-center">
        <!-- svelte-ignore a11y-missing-attribute -->
        <div class="gallery mb-5">
          <img id="avatar" class="border-solid rounded-lg border-gray-400 border-opacity-50" src="{images[currentIndex]}" height="350px" width="350px" alt="Avatar">
        </div>
  
        <!-- Flex container for buttons and input fields -->
        <div class="flex flex-col md:flex-row justify-center items-center w-full text-gray-400 mt-5">
          <!-- Left Arrow and Input Field -->
          <div class="flex items-center mb-2 md:mb-0">
            <button on:click={previous} class="mr-2">◀</button>
            <input
              class="input-field z-1 flex-grow"
              bind:value="{query}"
              on:keydown={handleKeyPress}
              placeholder="Enter query..."
            />
          </div>
  
          <!-- Right Button and Input Field -->
          <div class="flex items-center mt-2 md:mt-0">
            <input
              class="input-field z-1"
              type="number"
              bind:value="{page}"
              on:keydown={handleKeyPress}
              placeholder="Enter page..."
            />
            <button on:click={next} class="ml-2">▶</button>
          </div>
        </div>
      </div>
    </div>
  </html>
