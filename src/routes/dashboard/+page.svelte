<h1>Welcome to the dashboard</h1>
<script>
  import Profile from "$lib/Profile.svelte";
  
  import { supabase } from '$lib/supabaseClient'

  user.set(supabase.auth.user())
  
    supabase.auth.onAuthStateChange((state, session) => {
    	if (state == 'SIGNED_IN') {
    		user.set(session.user)	
    	} else {
    		user.set(false);
    	}
    })
</script>


<div class="container">
  <Profile />
</div>

<style>
  h1 {
    font-family: "Oswald", sans-serif;
    font-size: 3em;
    margin: 0;
    color: white;
  }
</style>