<script>
	import { onMount } from 'svelte';
	const serverUrl = import.meta.env.VITE_SERVER_URL

	
	
	

	
</script>




				<div class="outerdiv">
					<main class="blogcon">
						<h6><strong>History...</strong></h6>
					  
						<div class="blog-card">
						  <img class="thumbnail" src="https://res.cloudinary.com/dh753xlhq/image/upload/v1733129773/blogs/iugwubprp4lr2hi4wyar.jpg" alt="Thumbnail for Star wars" />
						  <div class="content">
							<h2>Star wars</h2>
							<p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Reprehenderit, officiis.</p>
						  </div>
						  <div class="interaction-bar">
							<span class="icon">👍 0</span>
							<!-- <span class="icon">💬 3</span> -->
							<!-- <span class="icon">🔗 Share</span> -->
							<span class="icon">👀 30k</span>
						  </div>
						</div>
					</main>
				  </div>



<style>
	h6 strong {
		position: sticky;
		top: 0px;
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
    .interaction-bar {
      display: flex;
      justify-content: space-around;
      margin-top: 8px;
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

    }


</style>
