<script>
    import { user } from '$lib/sessionStore'
    import { supabase } from '$lib/supabaseClient'
    import Auth from '$lib/Auth.svelte'
    import Profile from '$lib/Profile.svelte'
    import LoginForm from "$lib/loginForm.svelte";

  
    user.set(supabase.auth.user())
  
    supabase.auth.onAuthStateChange((_, session) => {
      user.set(session.user)
    })
  </script>
  
  <div class="container" style="padding: 50px 0 100px 0;">
    {#if $user}
    <Profile />
    {:else}
    <Auth />
    <br>
    <a href="https://temp-mail.org/">Get a temporary email address here to test it out!</a>
    {/if}
  </div>
  
  <div class="container">
    <LoginForm title="Login" />
  </div>
  
  <style>
    .container {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 60vh;
      width: 100%;
    }
  </style>