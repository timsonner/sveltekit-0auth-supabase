<!-- Code from https://github.com/rboddy/supabase-yt-auth -->
<script>
    import { supabase } from '$lib/supabaseClient'
    import { goto } from "$app/navigation";
    import { user } from '$lib/sessionStore'
    let email = "";
    let password = "";
    // let loading = true
    export let title;
  
    async function handleLogin() {
    //   if (title == "Login") {
        const { user, error } = await supabase.auth.signIn({
          email: email,
          password: password,
        });
        if (user) {
          goto("/dashboard");
        } else {
          console.log(error);
        }
    //   } else {
    //     const { user, error } = await supabase.auth.signUp({
    //       email: email,
    //       password: password,
    //     });
    //     if (user) {
    //       goto("/dashboard");
    //     } else {
    //       console.log(error);
    //     }
    //   }
    }

    // const handleLogin = async () => {
    //   try {
    //     loading = true
    //     const { user, error } = await supabase.auth.signIn({ 
    //         email: email,
    //         password: password 
    //     })
    //     if (user) {
    //       goto("/dashboard");
    //     }

    //     if (error) throw error
    //     alert(`Error: ${error}`)
    //   } catch (error) {
    //     alert(error.error_description || error.message)
    //   } finally {
    //     loading = false
    //   }
    // }

  </script>
  
  <div class="loginFormContainer">
    <h1>{title}</h1>
    <form class="loginForm" on:submit|preventDefault={handleLogin}>
      <input type="email" bind:value={email} placeholder="email@email.com" />
      <input type="password" bind:value={password} placeholder="Password" />
      <button type="submit">Login</button>
    </form>
    <a href="/signup">Not a Member? Sign up!</a>
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
    a {
      text-decoration: none;
      color: rgb(9, 227, 9);
      display: block;
      margin-top: 10px;
    }
  </style>
  