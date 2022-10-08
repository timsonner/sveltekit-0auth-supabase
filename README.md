## install project dependencies
> npm install

## create a .env
> VITE_SUPABASE_URL="YOUR_SUPABASE_URL"  
> VITE_SUPABASE_ANON_KEY="YOUR_SUPABASE_KEY"

## create supabase database
1. Go to app.supabase.com
2. Click on 'New Project'
3. Go to the SQL Editor page in the Dashboard
4. Click User Management Starter
5. Click Run

## copy keys to .env
1. Click 'Home' in the sidebar
2. Find Project URL and API Key
3. Paste the keys into the .env

## run project

> npm run dev -- --open
