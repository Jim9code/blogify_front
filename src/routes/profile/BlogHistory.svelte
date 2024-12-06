<script>
	import { onMount } from 'svelte';
  import Spinner from '../Spinner.svelte';
	const serverUrl = import.meta.env.VITE_SERVER_URL

	let bloghistory = []

	async function blogHistory() {
    const res = await fetch(`${serverUrl}/bloghistory`,{credentials:'include'})
    if(res.ok){
      bloghistory= await res.json()
    }
  }

  setInterval(() => {
    blogHistory()
  }, 3000);


  let loading = false
  let success = '';
  // handle delete
  async function deleteBlog(blogId) {
    loading = true
    const res = await fetch(`${serverUrl}/delete/${blogId}`,{
      method:'post',
      credentials:'include'
    })
    if(res.ok){
      const data = await res.json()
       success = data.success
       loading = false
       blogHistory()
    }
  }
	

	onMount(()=>{
    blogHistory()
  })
</script>




				<div class="outerdiv">
					<main class="blogcon">
            <div class="sticky">
              <h6><strong >History...</strong></h6>
              {#if success === ''}
                <div></div>
                 {:else}
                 <div style="margin-top: 10px;" class="alert alert-success">
                  {success}
              </div>
              {/if}
              {#if loading}
                 <Spinner/>
              {/if}
            </div>

            
					  {#each bloghistory as blog}
            <div class="blog-card">
						  <img class="thumbnail" src={blog.cloudinary_url} alt="Thumbnail for Star wars" />
						  <div class="content">

                 <ul class="dropdown">
                 <li class="nav-item dropdown">
                <a class="nav-link " href="/" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                  ⚙️
                </a>
                <ul class="dropdown-menu">
                  <!-- <li><a class="dropdown-item" href="/report">Report</a></li> -->
                  <li><a class="dropdown-item" href="/profile/{blog.id}">Edit</a></li>
                  <li><hr class="dropdown-divider"></li>
                  <li><a on:click={deleteBlog(blog.id)} class="dropdown-item" href="/profile">Delete</a></li>
                </ul>
              </li> 
                 </ul>


							<h2><a href="{blog.id}">{blog.blog_title}</a> </h2>
							<small>Written by You at {blog.created_at}</small>
						  </div>
						</div>
            {/each}
					</main>
				  </div>




<style>
.sticky{
  position: sticky;
  top: 50px;
  z-index: 2000;
}

  .dropdown{
    float: right;
    list-style: none;
  }


  h2 a{
    color: black;
    text-decoration: none;
    font-family: sans-serif;
  }
	h6 strong {
		color: orangered;
    padding: 10px;
    border-radius: 8px;
    background: white;
    border: 1px solid orangered;
	}
	
    .blogcon{
        width: 60%;
    }
    .outerdiv{
        width: 100%;
        display: flex;
        justify-content: center;
    }
    .blog-card {
      border: 1px solid #ddd;
      border-radius: 8px;
      overflow: hidden;
      margin: 16px 0;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      padding: 10px;
    }
    .thumbnail {
      width: 100%;
      height: 300px;
      object-fit: cover;
    }
    .content {
      padding: 16px;
    }

    @media only screen and (max-width:768px){
      .outerdiv{
        width: 100%;
      }
      .blogcon{
        width: 100%;
      }

    }


</style>
