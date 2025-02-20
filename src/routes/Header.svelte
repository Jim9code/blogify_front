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
      window.location.href = '/'
    }
  }



onMount(()=>{
  login_status();
})
</script>

<div class="outdiv">
    <nav class="navbar navbar-expand-lg">
        <div class="container">
          <a class="navbar-brand" href="/">WwwBlog</a>
          
          <div class="nav-right-items">
            <!-- profile pic -->
            <a class="profilePic" href="/profile">           
              <img src={profilePicture || DefaultProfile} alt="Profile picture" class="rounded-circle">
            </a>

            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
              <span class="navbar-toggler-icon"></span>
            </button>
          </div>

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

            <div class="auth-buttons">
              {#if authenticated}
                <button class="sign-button" on:click={logout}>Sign Out</button>
              {:else}
                <a class="sign-button" href="/profile">
                  <span class="sign-in">Sign In</span>
                  <span class="divider">|</span>
                  <span class="sign-up">Sign Up</span>
                </a>
              {/if}
            </div>
          </div>
        </div>
    </nav>

    <div class="notification-banner">
      <span>Note: only comment section is working for now</span>
    </div>
</div>

<style>
  .navbar {
    background-color: white;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    padding: 0.5rem 0;
  }

  .navbar-brand {
    font-family: 'Courier New', monospace;
    font-weight: bold;
    font-size: 1.5rem;
    color: #333;
  }

  .nav-right-items {
    display: flex;
    align-items: center;
    gap: 1rem;
  }

  .profilePic {
    display: flex;
    align-items: center;
  }

  .profilePic img {
    width: 40px;
    height: 40px;
    border: 2px solid #fff;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    transition: transform 0.2s;
  }

  .profilePic img:hover {
    transform: scale(1.1);
  }

  .sign-button {
    text-decoration: none;
    background-color: white;
    color: #333;
    font-family: sans-serif;
    font-weight: 600;
    border: 2px solid #e0e0e0;
    padding: 0.5rem 1rem;
    border-radius: 20px;
    transition: all 0.3s ease;
    cursor: pointer;
  }

  .sign-button:hover {
    background-color: #f8f8f8;
    border-color: #333;
  }

  .sign-in {
    color: #2ecc71;
  }

  .divider {
    margin: 0 0.5rem;
    color: #666;
  }

  .notification-banner {
    background-color: #fff3e0;
    color: #f57c00;
    text-align: center;
    padding: 0.5rem;
    font-weight: bold;
    font-size: 0.9rem;
  }

  .outdiv {
    position: sticky;
    top: 0;
    z-index: 1000;
    background-color: white;
  }

  @media only screen and (max-width: 768px) {
    .navbar {
      padding: 0.5rem 1rem;
    }

    .nav-right-items {
      gap: 0.5rem;
    }

    .profilePic img {
      width: 35px;
      height: 35px;
    }

    .sign-button {
      padding: 0.4rem 0.8rem;
      font-size: 0.9rem;
    }

    .navbar-brand {
      font-size: 1.2rem;
    }

    .notification-banner {
      font-size: 0.8rem;
    }
  }

  @media only screen and (max-width: 576px) {
    .container {
      padding: 0 0.5rem;
    }

    .auth-buttons {
      margin-top: 1rem;
      text-align: center;
    }
  }
</style>
