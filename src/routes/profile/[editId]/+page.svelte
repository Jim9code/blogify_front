<script>
    import Edit from "./Edit.svelte";
    import Login from '../../Login.svelte'
	import { onMount } from "svelte";
    const serverUrl = import.meta.env.VITE_SERVER_URL


    let logged_in = false
  async function login_status() {
    const res = await fetch(`${serverUrl}/protected`,{credentials:'include'})
    if(res.ok){
      const data = await res.json()
      logged_in = data.authenticated
      console.log(logged_in)
    }
  }
    onMount(()=>{
        login_status()
    })
</script>

<div>
    {#if logged_in}
    <Edit/>
      {:else}
        <Login/>
    {/if}
    
    
</div>