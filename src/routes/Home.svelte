<script>
  import { onMount } from "svelte";
  import Spinner from './Spinner.svelte'
	import Comment from "./Comment.svelte";

	const serverUrl = import.meta.env.VITE_SERVER_URL

  let comment = false
  let blogCommenntID = 0

    function toggleComment(blogcommentid){
      comment = comment === false

       blogCommenntID = blogcommentid
      
     
    //  console.log(comment)

    //  return comment

  }



    let blogs = [];
  let loading = false
    async function blog() {
      loading = true
    const res = await fetch(`${serverUrl}/blogs`,{credentials:'include'})
    if(res.ok){
      blogs= await res.json()
      loading = false
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
  
  
  
<div class="main">
  
  <div class="innerdiv">
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
          {#if loading}
             <Spinner/>
             {:else}
             {#if blogs.length <= 0}
             <center><h6>No search  result !</h6></center>
             {/if}
          {/if}
         

          {#each blogs as blog}
            <div class="blog-card">
              <a href="/{blog.id}">
                <img class="thumbnail" src={blog.cloudinary_url} alt="Thumbnail for {blog.blog_title}" />
              </a>
               <!-- smal pic -->
                         
                <div style="margin-top: 4px;">
                   <!-- svelte-ignore a11y-img-redundant-alt -->
                   <img src={blog.bloger_pic} crossorigin="anonymous" alt="Profile picture" width="30px" height="30px" loading="lazy" class="rounded-circle">
                  <small> {blog.bloger_username}</small>
                </div>
                
              <div class="content">
                
                <h6><a href="/{blog.id}">📖{blog.blog_title}</a></h6>
                <!-- <p>{blog.excerpt}</p> -->
              </div>
              <div class="interaction-bar">
                <span class="icon">👍 0</span>
                <span class="icon" on:click={toggleComment(blog.id)}>💬</span>
                <span class="icon">🔗 Share</span>
                <span class="icon">👀 0</span>
              </div> 
            </div>
          {/each}

           {#if comment}
           <Comment blogcommentId={blogCommenntID}/>
           {/if}


        </main>
      </div>
  </div>
</div>
 

  <style>

    .icon{
      cursor: pointer;
    }
   
.interaction-bar{
  display: flex;
  justify-content: space-around;
}
     

    h6 a{
      color: black;
      text-decoration: none;
    }


    .btn_search{
        background: rgba(50, 205, 50, 0.813);
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
        padding: 4px;
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
      margin: 20px 0;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      padding: 10px;
      background: white;
      
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

  
