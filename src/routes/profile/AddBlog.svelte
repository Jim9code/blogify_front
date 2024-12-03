<script>
    import { onMount } from "svelte";
    import Spinner from '../Spinner.svelte'
    const serverUrl = import.meta.env.VITE_SERVER_URL

  
    let blogTitle = "";
    let blogThumbnail = null;
    let blogThumbnailPreview = null; // Holds the preview URL
    let blogContent = "";
    let editorInstance = null;
  
    async function initializeEditor() {
      const ClassicEditor = (await import("@ckeditor/ckeditor5-build-classic")).default;
      const editorElement = document.querySelector("#editor");
      editorInstance = await ClassicEditor.create(editorElement, {
        toolbar: [
          "heading",
          "|",
          "bold",
          "italic",
          "link",
          "bulletedList",
          "numberedList",
          "|",
          "blockQuote",
          "undo",
          "redo",
        ],
      });
  
      editorInstance.model.document.on("change:data", () => {
        blogContent = editorInstance.getData();
      });
    }
  
    function handleThumbnailChange(event) {
      const file = event.target.files[0];
      if (file) {
        blogThumbnail = file;
  
        // Generate a preview URL
        const reader = new FileReader();
        reader.onload = (e) => {
          blogThumbnailPreview = e.target.result;
        };
        reader.readAsDataURL(file);
      } else {
        blogThumbnail = null;
        blogThumbnailPreview = null;
      }
    }

    let success = ''
     let added = false
    let loading = false
   async function submitBlog(event) {
     loading = true
      if (blogTitle && blogContent && blogThumbnail !==null) {
        event.preventDefault()
        
        const formData = new FormData();
    formData.append("blogTitle", blogTitle);
    formData.append("blogContent", blogContent);
    formData.append("image", blogThumbnail); // The key here is "image"

    try {
      const response = await fetch(`${serverUrl}/addblog`, {
        method: "POST",
        body: formData,
        credentials:'include'
      });

      if (response.ok) {
        const result = await response.json();
        loading = false
        blogTitle = ''
        blogContent = ''
        blogThumbnail = null
        added = true
        success = result.success
      } 
    } catch (err) {
      console.error("Network error:", err);
      alert("An error occurred while submitting the blog.");
    }
      
      } else {
        alert("Please fill in all fields.");
      }
    }
  
    onMount(() => {
      if (typeof window !== "undefined") {
        initializeEditor();
      }
    });
    
  </script>
  
  
  
  <div class="profile-container">
  
  
    <!-- Blog Creation -->
    <section>
      {#if loading}
        <Spinner/>
         {:else}
            {#if added}
            {#if success === ''}
            <div></div>
             {:else}
             <div class="alert alert-success">
              {success}
              </div>
                   {/if}
                 {:else}
                 <div class="form-group">
                  <label for="title">Blog Title:</label><br>
                  <input id="title" type="text" bind:value={blogTitle} placeholder="Enter blog title" />
                </div>
            
                <div class="form-group">
                  <label for="thumbnail">Thumbnail:</label><br>
                  <input id="thumbnail" type="file" accept="image/*" on:change={handleThumbnailChange} />
                  {#if blogThumbnailPreview}
                    <img
                      src={blogThumbnailPreview}
                      alt="Thumbnail Preview"
                      class="thumbnail-preview"
                    />
                  {/if}
                </div>
            
                <div class="form-group">
                  <label for="editor">Blog Content:</label>
                  <div id="editor"></div>
                </div>
            
                <button on:click={submitBlog}>Submit Blog</button>
            {/if}
      {/if}
      
    </section>
  </div>
  
  <style>
    .profile-container {
      padding: 20px;
      max-width: 800px;
      margin: auto;
    }
    .form-group {
      margin-bottom: 20px;
    }
    #editor {
      border: 1px solid #ddd;
      padding: 10px;
      min-height: 300px;
    }
    .thumbnail-preview {
      margin-top: 10px;
      max-width: 200px;
      max-height: 200px;
      border: 1px solid #ddd;
      display: block;
      object-fit: cover;
    }
    button {
      margin-top: 20px;
      padding: 10px 20px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }

    @media only screen and (max-width:768px){
     .profile-container{
      max-width: 400px;
     }

    }
  </style>