<script>
import {page} from '$app/stores'
import { onMount } from "svelte";
let blogId = $page.params.blogId;

	const serverUrl = import.meta.env.VITE_SERVER_URL

    let singleblog= [];
  
    async function singleBlog() {
    const res = await fetch(`${serverUrl}/${blogId}`,{credentials:'include'})
    if(res.ok){
      singleblog= await res.json()
    }
  }
	

	onMount(()=>{
    singleBlog()
  })


  </script>
  
  
  
<div>
  


    <div class="outerdiv">
        <main class="blogcon">
          {#each singleblog as blog}
            <div class="blog-card">
              <img class="thumbnail" src={blog.cloudinary_url} alt="Thumbnail for {blog.blog_title}" />
               <!-- smal pic -->
                      
                <div style="margin-top: 4px;">
                  <!-- svelte-ignore a11y-img-redundant-alt -->
                  <!-- <img src={blog.bloger_pic} crossorigin="anonymous" alt="Profile picture" width="30px" height="30px" loading="lazy" class="rounded-circle"> -->
                  <small>Posted by {blog.bloger_username}</small>
                </div>
            
              <div class="content">
                
                <h6>{blog.blog_title}</h6>
                <p>{@html blog.blog_content}</p>
              </div>
              <!-- <div class="interaction-bar">
                <span class="icon">👍 {blog.likes}</span>
                <span class="icon">💬 {blog.comments}</span>
                <span class="icon">🔗 Share</span>
                <span class="icon">👀 {blog.views}</span>
              </div> -->
            </div>
          {/each}
        </main>
      </div>
</div>
 

  <style>
    .profilePic{
        color: green;
        text-decoration: none;
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
    .content p{
      font-family: sans-serif;
    }
    .content h2{
      font-family: sans-serif;
    }

    .interaction-bar {
      display: flex;
      justify-content: space-around;
      margin-top: 4px;
      margin-bottom: 15px;
    }
    .icon {
      cursor: pointer;
      font-size: 18px;
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

  