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

  async function handleLike(blogId) {
    try {
      const res = await fetch(`${serverUrl}/like/${blogId}`, {
        method: 'POST',
        credentials: 'include'
      });
      
      if (res.ok) {
        const data = await res.json();
        blogs = blogs.map(blog => {
          if (blog.id === blogId) {
            return { ...blog, likes: data.likes };
          }
          return blog;
        });
      } else {
        const errorData = await res.json();
        console.error('Like failed:', errorData.error);
      }
    } catch (error) {
      console.error('Error in handleLike:', error);
    }
  }

  async function handleShare(blog) {
    if (navigator.share) {
      try {
        await navigator.share({
          title: blog.blog_title,
          text: `Check out this blog post: ${blog.blog_title}`,
          url: window.location.origin + '/' + blog.id
        });
      } catch (err) {
        console.error('Share failed:', err);
      }
    } else {
      const url = window.location.origin + '/' + blog.id;
      await navigator.clipboard.writeText(url);
      alert('Link copied to clipboard!');
    }
  }

  async function incrementView(blogId) {
    try {
      const res = await fetch(`${serverUrl}/view/${blogId}`, {
        method: 'POST',
        credentials: 'include'
      });
      
      if (res.ok) {
        const data = await res.json();
        blogs = blogs.map(blog => {
          if (blog.id === blogId) {
            return { ...blog, views: data.views };
          }
          return blog;
        });
      } else {
        const errorData = await res.json();
        console.error('View count failed:', errorData.error);
      }
    } catch (error) {
      console.error('Error in incrementView:', error);
    }
  }
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
              <a href="/{blog.id}" on:click={() => incrementView(blog.id)}>
                <img class="thumbnail" src={blog.cloudinary_url} alt="Thumbnail for {blog.blog_title}" />
              </a>
               <!-- smal pic -->
                         
                <div style="margin-top: 4px;">
                   <!-- svelte-ignore a11y-img-redundant-alt -->
                   <img src={blog.bloger_pic} crossorigin="anonymous" alt="Profile picture" width="30px" height="30px" loading="lazy" class="rounded-circle">
                  <small> {blog.bloger_username}</small>
                </div>
                
              <div class="content">
                
                <h6><a href="/{blog.id}" on:click={() => incrementView(blog.id)}>📖{blog.blog_title}</a></h6>
                <!-- <p>{blog.excerpt}</p> -->
              </div>
              <div class="interaction-bar">
                <span class="icon" on:click={() => handleLike(blog.id)}>👍 {blog.likes ?? 0}</span>
                <span class="icon" on:click={() => toggleComment(blog.id)}>💬 {blog.comments_count ?? 0}</span>
                <span class="icon" on:click={() => handleShare(blog)}>🔗 Share</span>
                <span class="icon">👀 {blog.views ?? 0}</span>
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

    .icon {
      cursor: pointer;
      font-size: 0.9rem;
      padding: 4px;
    }
   
    .interaction-bar {
      display: flex;
      justify-content: space-around;
      padding: 8px 0;
    }
     
    h6 {
      margin: 8px 0;
      font-size: 1rem;
    }

    h6 a {
      color: black;
      text-decoration: none;
    }

    .btn_search {
        background: rgba(50, 205, 50, 0.813);
        border-radius: 8px;
        padding: 7px 15px;
        font-weight: bold;
        border: none;
        font-size: 1rem;
        transition: background 0.2s;
    }

    .btn_search:hover {
        background: rgba(50, 205, 50, 0.9);
    }

    form {
      display: flex;
      flex-direction: row;
      align-items: center;
      gap: 10px;
      width: 100%;
      max-width: 600px;
      padding: 0 15px;
    }

    .searchinput {
      padding: 8px 12px;
      width: 100%;
      border-radius: 8px;
      border: 1px solid #ddd;
      font-size: 1rem;
    }

    .searchinput:focus {
      border: 1px solid rgba(50, 205, 50, 0.696);
      outline: none;
    }

    .searchdiv {
        background: white;
        padding: 15px 0;
        position: sticky;
        top: 0px;
        z-index: 2000;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }

    .blogcon {
        width: 90%;
        max-width: 800px;
        margin: 0 auto;
        padding: 0 15px;
    }

    .outerdiv {
        width: 100%;
    }

    .blog-card {
      border-radius: 12px;
      overflow: hidden;
      margin: 20px 0;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
      padding: 15px;
      background: white;
      transition: transform 0.2s;
    }

    .blog-card:hover {
      transform: translateY(-2px);
    }

    .thumbnail {
      width: 100%;
      height: 250px;
      object-fit: cover;
      border-radius: 8px;
    }

    .content {
      padding: 12px 0;
    }

    .rounded-circle {
      border-radius: 50%;
      margin-right: 8px;
    }

    @media only screen and (max-width: 768px) {
      .blogcon {
        width: 100%;
        padding: 0 10px;
      }

      form {
        flex-direction: column;
        padding: 0 10px;
      }

      .searchinput {
        width: 100%;
      }

      .btn_search {
        width: 100%;
        margin-top: 8px;
        padding: 8px;
      }

      .thumbnail {
        height: 200px;
      }

      .content {
        padding: 8px 0;
      }

      h6 {
        font-size: 0.9rem;
      }
    }

    @media only screen and (max-width: 480px) {
      .blog-card {
        padding: 10px;
        margin: 15px 0;
      }

      .thumbnail {
        height: 180px;
      }

      .icon {
        font-size: 0.8rem;
      }
    }
  </style>

  
