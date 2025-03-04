<script>
	import { onMount } from "svelte";
  const serverUrl = import.meta.env.VITE_SERVER_URL;

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

<header class="header">
  <div class="container">
    <nav class="nav">
      <a href="/" class="logo">WwwBlog</a>

      <div class="nav-right">
        <a href="/profile" class="profile-link">           
          <img src={profilePicture || DefaultProfile} alt="Profile" class="profile-img">
        </a>

        <div class="auth">
          {#if authenticated}
            <button class="auth-button logout" on:click={logout}>
              <span>Sign Out</span>
            </button>
          {:else}
            <a class="auth-button login" href="/profile">
              <span>Sign In</span>
              <span class="divider">|</span>
              <span>Sign Up</span>
            </a>
          {/if}
        </div>
      </div>
    </nav>
  </div>
</header>

<style>
  .header {
    position: sticky;
    top: 0;
    z-index: 1000;
    background: white;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }

  .container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 1rem;
  }

  .nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 40px;
  }

  .logo {
    font-family: 'Poppins', sans-serif;
    font-size: 1.3rem;
    font-weight: 700;
    color: #2d3436;
    text-decoration: none;
    letter-spacing: -0.5px;
    transition: color 0.3s ease;
  }

  .logo:hover {
    color: #0984e3;
  }

  .nav-right {
    display: flex;
    align-items: center;
    gap: 1.5rem;
  }

  .profile-link {
    display: flex;
    align-items: center;
  }

  .profile-img {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    object-fit: cover;
    border: 2px solid #fff;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

  .profile-img:hover {
    transform: scale(1.05);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
  }

  .auth-button {
    display: inline-flex;
    align-items: center;
    padding: 0.35rem 0.9rem;
    border-radius: 25px;
    font-weight: 600;
    font-size: 0.85rem;
    text-decoration: none;
    transition: all 0.3s ease;
    border: none;
    cursor: pointer;
  }

  .login {
    background: linear-gradient(135deg, #6366f1 0%, #4f46e5 100%);
    color: white;
  }

  .login:hover {
    background: linear-gradient(135deg, #4f46e5 0%, #4338ca 100%);
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(79, 70, 229, 0.2);
  }

  .logout {
    background: white;
    color: #4f46e5;
    border: 2px solid #4f46e5;
  }

  .logout:hover {
    background: #4f46e5;
    color: white;
    transform: translateY(-1px);
  }

  .divider {
    margin: 0 0.5rem;
    color: rgba(255, 255, 255, 0.8);
  }

  @media (max-width: 768px) {
    .nav {
      height: 35px;
    }

    .logo {
      font-size: 1.2rem;
    }

    .nav-right {
      gap: 1rem;
    }

    .profile-img {
      width: 28px;
      height: 28px;
    }

    .auth-button {
      padding: 0.3rem 0.7rem;
      font-size: 0.8rem;
    }
  }

  @media (max-width: 480px) {
    .container {
      padding: 0 0.8rem;
    }

    .logo {
      font-size: 1.3rem;
    }

    .auth-button {
      padding: 0.4rem 0.8rem;
      font-size: 0.85rem;
    }
  }
</style>
