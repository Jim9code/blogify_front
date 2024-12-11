<script>
	import { onMount } from "svelte";
  import Profile from "./Profile.svelte";
  import Login from "../Login.svelte";
	import Spinner from "../Spinner.svelte";
  const serverUrl = import.meta.env.VITE_SERVER_URL


let loading = false
  let logged_in = false;
  async function login_status() {
    loading = true
    const res = await fetch(`${serverUrl}/protected`,{credentials:'include'})
    if(res.ok){
      const data = await res.json()
      logged_in = data.authenticated
      loading = false
    }
  }


onMount(()=>{
  login_status();
})
</script>

<div>
  {#if logged_in}
    <Profile/>
      {:else}
      {#if loading}
        <Spinner/>
         {:else}
         <Login/>
      {/if}
  {/if}

</div>
