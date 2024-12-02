<script>
	import { onMount } from "svelte";
  import Home from "./Home.svelte";
  import Login from "./Login.svelte";
  const serverUrl = import.meta.env.VITE_SERVER_URL




  let logged_in = false;

  async function login_status() {
    const res = await fetch(`${serverUrl}/protected`,{credentials:'include'})
    if(res.ok){
      const data = await res.json()
      logged_in = data.authenticated
    }
  }


onMount(()=>{
  login_status();
})
</script>

<div>
    <Home/>
</div>
