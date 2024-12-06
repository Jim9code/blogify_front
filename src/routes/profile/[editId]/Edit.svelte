<script>
    import {page} from '$app/stores'
    import { onMount } from "svelte";
    const serverUrl = import.meta.env.VITE_SERVER_URL
    let editId = $page.params.editId

  
    // edit content
    let success = ''
   let editContent = ''
  async function editBlog() {
    const res = await fetch(`${serverUrl}/profile/blog/${editId}`,{credentials:'include'})
    if(res.ok){
      const data = await res.json()
      editContent = data.blog_content
    }
  }

    
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

      editorInstance.setData(editContent)
  
      editorInstance.model.document.on("change:data", () => {
        blogContent = editorInstance.getData();
      });
    }
  

//   submit content
 async function submitBlog() {
    console.log(blogContent)
    const res = await fetch(`${serverUrl}/updateEdit`,{
        method:'post',
        headers:{'Content-Type':'application/json'},
        body:JSON.stringify({editId,blogContent}),
        credentials:'include'
    })
    if(res.ok){
      const data = await res.json()
    
    }
  }
   
    onMount(() => {
        editBlog()
      if (typeof window !== "undefined") {
        initializeEditor();
      }
    });
    
  </script>
  
  
  
  <div class="profile-container">
  
  
    <!-- Blog Creation -->
    <section>
     
            
            {#if success === ''}
            <div></div>
             {:else}
             <div class="alert alert-success">
              {success}
              </div>
                   {/if}
                
            
                <div class="form-group">
                  <label for="editor">Edit Blog Content: <small>Note:if you save an empty content it will update as empty!</small></label>
                  <div id="editor"></div>
                </div>
            
                <button on:click={submitBlog}>Submit Blog</button>

    
      
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