# Signup or login using Supabase Auth

## This project is using the latest SvelteKit as of 10/08/2022
Visit the latest deployment of this repo  
<https://sveltekit-0auth-supabase.vercel.app/>
## install project dependencies
> npm install

## create .env
> VITE_SUPABASE_URL="YOUR_SUPABASE_URL"  
> VITE_SUPABASE_ANON_KEY="YOUR_SUPABASE_KEY"

## create supabase database
1. Go to app.supabase.com
2. Click on 'New Project'
3. Go to the SQL Editor page in the Dashboard
4. Click User Management Starter
5. Click Run

## paste keys to .env
1. Click 'Home' in the sidebar
2. Find Project URL and API Key
3. Paste the keys into the .env

## run project

> npm run dev -- --open

partial code source:
<https://github.com/rboddy/supabase-yt-auth>  
<https://supabase.com/docs/guides/with-sveltekit>
