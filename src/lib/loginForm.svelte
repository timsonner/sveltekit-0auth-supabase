<!-- Code from https://github.com/rboddy/supabase-yt-auth -->
<script>
	import { supabase } from '$lib/supabaseClient';
	import { goto } from '$app/navigation';

	let email = '';
	let password = '';
	export let title;

	async function handleLogin() {
		const { user, error } = await supabase.auth.signIn({
			email: email,
			password: password
		});
		if (user) {
			goto('/dashboard');
		} else {
			console.log(error);
		}
	}
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
      background-color: #4a4444;
    }
    .loginFormContainer > h1 {
      font-family: "Courier New", monospace;
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
      background-color: rgb(30, 30, 30);
    }
    input:focus {
      outline: none;
    }

    button {
      width: 100px;
      height: 40px;
      border: none;
      background-color: rgb(63, 63, 63);
      font-size: 1em;
      border-radius: 15px;
      box-shadow: 2px 4px 4px rgba(0, 0, 0, 0.25);
    }
    a {
      text-decoration: none;
      color:rgb(0, 150, 252);
      display: block;
      margin-top: 10px;
    }
    h1 {
        color:rgb(213, 213, 213)
    }
</style>
