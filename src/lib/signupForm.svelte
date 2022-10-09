<!-- partial code source: https://github.com/rboddy/supabase-yt-auth -->
<script>
    import { supabase } from '$lib/supabaseClient'
    import { goto } from "$app/navigation";
    import { user } from '$lib/sessionStore'
    let email = "";
    let password = "";
    // let loading = true
    export let title;
  
    async function addProfile() {
      try {
        // loading = true
        const { user } = await supabase.auth.signUp({
          email: email,
          password: password,
        });
  
        const updates = {
       id: user.id,
        //   username,
          email,
          password,
        //   website,
        //   avatar_url,
        //   updated_at: new Date(),
        }
  
        let { error } = await supabase.from('profiles').upsert(updates, {
          returning: 'minimal', // Don't return the value after inserting
        })  
        if (error) throw error
      } catch (error) {
        alert(error.message)
      } finally {
        // loading = false
      }
      goto("/dashboard");
    }
  </script>
  
  <div class="loginFormContainer">
    <h1>{title}</h1>
    <form class="loginForm" on:submit|preventDefault={addProfile}>
      <input type="email" bind:value={email} placeholder="email@email.com" />
      <input type="password" bind:value={password} placeholder="Password" />
      <button type="submit">Sign Up</button>
    </form>
  </div>
  
  <style>
    .loginFormContainer {
      padding: 30px;
      border-radius: 15px;
      box-shadow: 2px 4px 4px rgba(0, 0, 0, 0.25);
      background-color: #808080;
    }
    .loginFormContainer > h1 {
      font-family: "Oswald", sans-serif;
      font-size: 3em;
      margin: 0;
    }
    .loginForm {
      display: flex;
      flex-direction: column;
      width: 30%;
    }
    .loginForm * {
      margin-top: 10px;
    }
    input {
      width: 300px;
      height: 40px;
      border: none;
      border-radius: 15px;
      padding: 10px;
      box-sizing: border-box;
    }
    input:focus {
      outline: none;
    }
    button {
      width: 100px;
      height: 40px;
      border: none;
      background-color: rgb(9, 227, 9);
      font-size: 1em;
      border-radius: 15px;
      box-shadow: 2px 4px 4px rgba(0, 0, 0, 0.25);
    }
    /* a {
      text-decoration: none;
      color: rgb(9, 227, 9);
      display: block;
      margin-top: 10px;
    } */
  </style>
  