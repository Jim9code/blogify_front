<script>
	import { onMount } from 'svelte';
	import BlogHistory from './BlogHistory.svelte';
	import AddBlog from './AddBlog.svelte';
	const serverUrl = import.meta.env.VITE_SERVER_URL

	
	let agent = {
		name: 'John Doe',
		email: 'johndoe@example.com',
		phone: '+234 812 345 6789',
		profilePic: '/test.jpg.jpg',
		rating: 4.5, // Average rating out of 5 stars
		reviews: [
			{ reviewer: 'Jane Smith', text: 'Great agent, very helpful!', rating: 5 },
			{ reviewer: 'David Mark', text: 'Helped me get a great deal.', rating: 4 },
			{ reviewer: 'Jeth', text: ' great deal.', rating: 4 }
		]
	};

	

	
	let zoomImage = null;

	// Zoom handlers
	const zoomIn = (image) => {
		zoomImage = image;
	};

	const closeZoom = () => {
		zoomImage = null;
	};


	let add_new_blog = false
	

	
</script>

<div  class="container-fluid bg-white">
<main  class="container-fluid container-md " id="main">
	<!-- Horizontal Image Gallery with Scroll -->

	<!-- Agent Section -->

		<div id="info" class="col-md-12 text-center">
			<!-- svelte-ignore a11y-img-redundant-alt -->
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
			<img
				style="width: 80px; height:80px;"
				src={agent.profilePic}
				alt="Agent Profile Picture"
				class="rounded-circle agent-profile-img"
				on:click={()=> zoomIn(agent.profilePic)}
			/>
			<h5 class="mt-2">{agent.name}</h5>

			{#if add_new_blog}
			<button on:click={()=>{add_new_blog = false}} class="bg-danger">Close</button>
			  {:else}
			  <button on:click={()=>{add_new_blog = true}}>Add new blog</button>
			{/if}
			

			<!-- history Section -->
			<div class="reviews mt-4 text-start p-2" id="reviewdiv">
                {#if add_new_blog}
                    <AddBlog/>
                {/if}
				<BlogHistory/>
			</div>
		</div>

		
</main>
<!-- Zoom Modal -->
{#if zoomImage}
	<!-- svelte-ignore a11y-click-events-have-key-events -->
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<div class="zoom-modal" on:click={closeZoom}>
		<!-- svelte-ignore a11y-img-redundant-alt -->
		<img src={zoomImage} alt="Zoomed Image" />
	</div>
{/if}
</div>



<style>
	h6 strong {
		position: sticky;
		top: 0px;
	}
	/* .cover-photo-container {
		position: relative;
		width: 100%;
		height: 200px;
		overflow: hidden;
		background-color: #ddd;
	}

	.cover-photo {
		width: 100%;
		height: 100%;
		object-fit: cover;
		cursor: pointer;
	} */

 .zoom-modal {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: rgba(0, 0, 0, 0.8);
		display: flex;
		align-items: center;
		justify-content: center;
		z-index: 1000;
		cursor: pointer;
	}

	.zoom-modal img {
		max-width: 90%;
		max-height: 90%;
		border-radius: 10px;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
	}

	#info {
		background: white;
		border-radius: 8px;
		position: relative;
		margin-top: 10px;
	}

	#main {
    background: white;
    display: flex;
    justify-content: center;
	width: 100%;
	

	}



	.agent-profile-img {
		width: 150px;
		height: 150px;
		object-fit: cover;
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
	  #main{
		margin: 0px;
		padding-right: 0px;
		padding-left: 0px;
		
	  }

    }


</style>
