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
  let showSearch = false
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
      <form on:submit|preventDefault={search} class="search-form">
        <div class="search-container">
          <!-- Desktop Search (always visible) -->
          <div class="desktop-search">
            <div class="input-wrapper">
              <span class="search-icon-prefix">🔍</span>
              <input 
                bind:value={searchWord} 
                class="searchinput" 
                type="search" 
                placeholder="Search blogs..." 
              >
              {#if searchWord}
                <button type="button" class="clear-button" on:click={() => {
                  searchWord = '';
                  blog();
                }}>×</button>
              {/if}
            </div>
          </div>

          <!-- Mobile Search (collapsible) -->
          <div class="mobile-search">
            {#if showSearch}
              <div class="input-group">
                <div class="input-wrapper">
                  <span class="search-icon-prefix">🔍</span>
                  <input 
                    bind:value={searchWord} 
                    class="searchinput" 
                    type="search" 
                    placeholder="Search blogs..." 
                    autofocus
                  >
                </div>
                <div class="mobile-buttons">
                  <button type="submit" class="search-button">Search</button>
                  <button type="button" class="cancel-button" on:click={() => {
                    showSearch = false;
                    searchWord = '';
                    blog();
                  }}>Cancel</button>
                </div>
              </div>
            {:else}
              <button type="button" class="search-icon" on:click={() => showSearch = true}>
                🔍
              </button>
            {/if}
          </div>
        </div>
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
        padding: 8px 0;
        position: sticky;
        top: 40px;
        z-index: 1999;
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

    .desktop-search {
      display: flex;
      align-items: center;
      width: 100%;
      gap: 10px;
    }

    .mobile-search {
      display: none;
    }

    @media (max-width: 768px) {
      .desktop-search {
        display: none;
      }

      .mobile-search {
        display: flex;
        width: 100%;
      }

      .searchdiv {
        top: 35px;
        padding: 6px 0;
      }

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

    .search-form {
      display: flex;
      justify-content: center;
      align-items: center;
      max-width: 1200px;
      margin: 0 auto;
      padding: 0 15px;
    }

    .search-container {
      width: 100%;
      max-width: 500px;
      margin: 0 auto;
      display: flex;
      justify-content: center;
    }

    .input-wrapper {
      position: relative;
      width: 100%;
      display: flex;
      align-items: center;
    }

    .search-icon-prefix {
      position: absolute;
      left: 12px;
      color: #666;
      font-size: 0.9rem;
    }

    .searchinput {
      width: 100%;
      padding: 8px 35px 8px 35px;
      border-radius: 20px;
      border: 2px solid #eee;
      font-size: 0.95rem;
      transition: all 0.2s ease;
      background: #f8f9fa;
    }

    .searchinput:focus {
      outline: none;
      border-color: #4f46e5;
      background: white;
      box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.1);
    }

    .clear-button {
      position: absolute;
      right: 10px;
      background: none;
      border: none;
      font-size: 1.2rem;
      color: #666;
      cursor: pointer;
      padding: 0 5px;
      transition: color 0.2s ease;
    }

    .clear-button:hover {
      color: #333;
    }

    .desktop-search {
      width: 100%;
      display: flex;
      justify-content: center;
    }

    .mobile-search {
      display: none;
    }

    .search-icon {
      background: none;
      border: none;
      font-size: 1.2rem;
      cursor: pointer;
      padding: 8px;
      border-radius: 50%;
      transition: background-color 0.2s ease;
    }

    .search-icon:hover {
      background-color: rgba(0, 0, 0, 0.05);
    }

    @media (max-width: 768px) {
      .desktop-search {
        display: none;
      }

      .mobile-search {
        display: flex;
        width: 100%;
        justify-content: center;
      }

      .search-container {
        max-width: 90%;
      }

      .searchdiv {
        top: 35px;
        padding: 6px 0;
      }

      .searchinput {
        font-size: 0.9rem;
        padding: 7px 30px 7px 30px;
      }
    }

    @media (max-width: 480px) {
      .search-container {
        max-width: 95%;
      }

      .searchinput {
        font-size: 0.85rem;
      }
    }

    .input-group {
      width: 100%;
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    .mobile-buttons {
      display: flex;
      gap: 8px;
    }

    .search-button, .cancel-button {
      padding: 6px 12px;
      border-radius: 16px;
      border: none;
      font-size: 0.9rem;
      cursor: pointer;
      transition: all 0.2s ease;
    }

    .search-button {
      background: #4f46e5;
      color: white;
      flex: 1;
    }

    .search-button:hover {
      background: #4338ca;
    }

    .cancel-button {
      background: #e5e7eb;
      color: #4b5563;
      padding: 6px 15px;
    }

    .cancel-button:hover {
      background: #d1d5db;
    }

    @media (max-width: 768px) {
      .input-group {
        padding: 0 10px;
      }

      .mobile-buttons {
        margin-top: 4px;
      }

      .search-button, .cancel-button {
        font-size: 0.85rem;
        padding: 6px 12px;
      }
    }

    @media (max-width: 480px) {
      .mobile-buttons {
        margin-top: 6px;
      }

      .search-button, .cancel-button {
        font-size: 0.8rem;
        padding: 5px 10px;
      }
    }
  </style>

  
