<script>
	import { onMount } from "svelte";
  const serverUrl = import.meta.env.VITE_SERVER_URL

  // login
let authenticated = false;
let profilePicture = 'https://static.vecteezy.com/system/resources/previews/009/734/564/original/default-avatar-profile-icon-of-social-media-user-vector.jpg'
let DefaultProfile = 'https://static.vecteezy.com/system/resources/previews/009/734/564/original/default-avatar-profile-icon-of-social-media-user-vector.jpg'

async function login_status() {
    const res = await fetch(`${serverUrl}/protected`,{credentials:'include'})
    if(res.ok){
      const data = await res.json()
      profilePicture = data.profile_pic
      authenticated = data.authenticated
    }
  }

  //LOGout
  async function logout() {
    const res = await fetch(`${serverUrl}/logout`,{credentials:'include'})
    if(res.ok){
      const data = await res.json()
    }
  }



onMount(()=>{
  login_status();
})
</script>

<div class="outdiv">
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
        <div class="container-fluid">
          <a class="navbar-brand" href="/">Blogify</a>
          <!-- profile pic -->
          <a class="profilePic"  href="/profile">           
            <div>
              <!-- svelte-ignore a11y-img-redundant-alt -->
              <img src={profilePicture || DefaultProfile}  alt="Profile picture" width="40px" height="40px" class="rounded-circle">
            </div>
            </a>

          <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
          </button>
          <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
              <li class="nav-item">
                <!-- <a class="nav-link active" aria-current="page" href="/cart">Cart</a> -->
              </li>
              <li class="nav-item">
                <!-- <a class="nav-link" href="/">Link</a> -->
              </li>
              <!-- <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" href="/" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                  <-- Dropdown -->
                <!-- </a> -->
                <!-- <ul class="dropdown-menu"> -->
                  <!-- <li><a class="dropdown-item" href="/report">Report</a></li> -->
                  <!-- <li><a class="dropdown-item" href="/">Another action</a></li> -->
                  <!-- <li><hr class="dropdown-divider"></li> -->
                  <!-- <li><a class="dropdown-item" href="/receipt">Something else here</a></li> -->
                <!-- </ul> -->
              <!-- </li>  -->
            </ul>

            {#if authenticated}
            <a class="sign" on:click={()=>{logout()}} href="/profile">           
               Sign Out
            </a>
                {:else}
                <a class="sign" href="/profile">           
                 <span style="color: green;"> Sign In </span>| Sign Up
                 </a>
          {/if}

           
           

           
          </div>
        </div>
      </nav>


     
     
</div>

<style>
  .profilePic{
     margin-left: 1px;

  }

  .sign{
    text-decoration: none;
    color: orangered;
    font-family: sans-serif;
    font-weight: bolder;
    border: 1px solid black;
    padding-left: 6px;
    padding-right: 6px;
    border-radius: 8px;
    
  }

  .outdiv{
    position: sticky;
    top: 0px;
    z-index: 1000;
  }
  @media only screen and (max-width:768px){
      .profilePic div img{
          width: 40px;
          height: 40px;
      }
    }
    
  </style>
