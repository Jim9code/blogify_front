<script>
	import { onMount } from 'svelte';
	import BlogHistory from './BlogHistory.svelte';
	import AddBlog from './AddBlog.svelte';
	const serverUrl = import.meta.env.VITE_SERVER_URL

	
	let user = {
		name: 'John Doe',
		profilePic: '/test.jpg.jpg',
	};
// get profile info
async function profileInfo() {
    const res = await fetch(`${serverUrl}/profileinfo`,{credentials:'include'})
    if(res.ok){
      const data = await res.json()
      user.name = data.username
	  user.profilePic = data.profile_picture

    }
  }
	

	
	

	let add_new_blog = false
	
onMount(()=>{
	profileInfo()
})
	
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
				src={user.profilePic}
				alt="Agent Profile Picture"
				class="rounded-circle agent-profile-img"
			/>
			<h5 class="mt-2">{user.name}</h5>

			<div class="closebtn">
			 {#if add_new_blog}
			<button on:click={()=>{add_new_blog = false}}> <span>close</span> </button>
			  {:else}
			  <button on:click={()=>{add_new_blog = true}}>Add new blog</button>
			{/if}
			</div>

			

			<!-- history Section -->
			<div class="reviews mt-4 text-start p-2" id="reviewdiv">
                {#if add_new_blog}
                    <AddBlog/>
                {/if}
				<BlogHistory/>
			</div>
		</div>

		
</main>
</div>



<style>
	
.closebtn button{
    border-radius: 8px;
	background: transparent;
	color: green;
	
}
.closebtn button span{
	color: orangered;
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

    

    @media only screen and (max-width:768px){

	  #main{
		margin: 0px;
		padding-right: 0px;
		padding-left: 0px;
		
	  }

    }


</style>
