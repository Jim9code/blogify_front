<script>
  import { onMount } from "svelte";
  
	const serverUrl = import.meta.env.VITE_SERVER_URL

    let blogs = [];
  
    async function blog() {
    const res = await fetch(`${serverUrl}/blogs`,{credentials:'included'})
    if(res.ok){
      blogs= await res.json()
    }
  }

  //  handle  search
  
  let searchWord = ''
  async function search() {
    if(searchWord !== ''){
      const res = await fetch(`${serverUrl}/search`,{
      method:'post',
      headers:{'Content-Type':'application/json'},
      body:JSON.stringify({searchWord}),
      credentials:'include'
    })
    if(res.ok){
      blogs= await res.json()
    }
  }else{
    blog()
  }
  }
	

	onMount(()=>{
    blog()
  })

   

  </script>
  
  
  
<div>
  
    <div class="searchdiv">
      <form on:submit|preventDefault={search} >
        <center>
            <input bind:value={searchWord} class="searchinput" type="search" placeholder="🔍 Search" > 
            <button class="btn_search" type="submit">Search</button>
        </center>
      </form>
    </div>

    <div class="outerdiv">
        <main class="blogcon">
          {#if blogs.length <= 0}
          <center><h6>No search  result !</h6></center>
          {/if}

          {#each blogs as blog}
            <div class="blog-card">
              <a href="/{blog.id}">
                <img class="thumbnail" src={blog.cloudinary_url} alt="Thumbnail for {blog.blog_title}" />
              </a>
               <!-- smal pic -->
                         
                <div style="margin-top: 4px;">
                  <small>Posted by {blog.bloger_username}</small>
                </div>
                
              <div class="content">
                
                <h6><a href="/{blog.id}">{blog.blog_title}</a></h6>
                <!-- <p>{blog.excerpt}</p> -->
              </div>
              <!-- <div class="interaction-bar">
                <span class="icon">👍 0</span>
                <span class="icon">💬 0</span>
                <span class="icon">🔗 Share</span>
                <span class="icon">👀 0</span>
              </div> -->
            </div>
          {/each}
        </main>
      </div>
</div>
 

  <style>
    h6 a{
      color: black;
      text-decoration: none;
    }


    .btn_search{
        background: rgba(50, 205, 50, 0.1);
        border-radius: 8px;
        padding: 7px;
        font-weight: bolder;
        border: none;
        font-size: 20px;
    }

  form{
  display: flex;
  justify-content: space-between;
}

  .searchinput{
  padding: 7.5px;
  width: 70%;
  border-radius: 8px;
  }.searchinput:focus{
    border: 1px solid rgba(50, 205, 50, 0.696);
  }
    .searchdiv{
        display: flex;
        justify-content: center;
        padding: 30px;
        position: sticky;
        top: 0px;
        z-index: 2000;
    }
    .blogcon{
        width: 50%;
    }
    .outerdiv{
        width: 100%;
        display: flex;
        justify-content: center;
    }
    .blog-card {
      /* border: 1px solid #ddd; */
      border-radius: 8px;
      overflow: hidden;
      margin: 16px 0;
      /* box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); */
      padding: 10px;
    }
    .thumbnail {
      width: 100%;
      height: 300px;
      object-fit: cover;
    }
    .content {
      padding: 16px;
      padding-bottom: 2px;
    }


    @media only screen and (max-width:768px){
      .outerdiv{
        width: 100%;
      }
      .blogcon{
        width: 100%;
      }
      .searchinput{
        padding: 5px;
        padding-left: 5px;
      }
      .btn_search{
        margin-top: 5px;
        font-size: 15px;

      }
    }
  </style>

  
