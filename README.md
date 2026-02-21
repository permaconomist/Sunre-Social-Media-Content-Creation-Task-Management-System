# Sunre-Social-Media-Content-Creation-Task-Management-System
A multi user social media content creation task managment app

Sunre AdContent Task Management System

Sunre AdContent Task Management System is a high-performance content operations journal and sync tool designed for creative teams. It bridges the gap between content creators and leadership by providing real-time progress tracking, AI-workflow logging, and a dedicated CEO/Admin monitor with a built-in user approval system.

🚀 Key Features

Approval Workflow: Secure registration system where new accounts remain in a "Pending" state until manually authorized by an administrator.

Creator Journal: Log daily campaign progress, current stage (Briefing, AI Gen, Editing, Review), and specific objectives.

CEO Monitor: High-level dashboard for leadership to track "On-Track" vs "At-Risk" campaigns and identify team blockers immediately.

Real-time Sync: Powered by Supabase for instant updates across all devices.

Glassmorphic UI: A premium, dark-mode interface built with Tailwind CSS.

🛠️ Deployment Instructions

1. Database Setup (Supabase)

Create a new project at Supabase.

Navigate to the SQL Editor in your Supabase dashboard.

Run the following SQL to set up the necessary tables and security policies:

-- Create Profiles Table
CREATE TABLE public.profiles (
  id uuid REFERENCES auth.users ON DELETE CASCADE PRIMARY KEY,
  name text,
  email text,
  role text DEFAULT 'creator',
  status text DEFAULT 'pending',
  created_at timestamptz DEFAULT now()
);

-- Create Sessions Table
CREATE TABLE public.sessions (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id uuid REFERENCES auth.users(id),
  campaign text,
  stage text,
  ceo_status text,
  objective text,
  blockers text,
  created_at timestamptz DEFAULT now()
);

-- Enable Row Level Security
ALTER TABLE public.profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.sessions ENABLE ROW LEVEL SECURITY;

-- Policies
CREATE POLICY "Users can view own profile" ON public.profiles FOR SELECT USING (auth.uid() = id);
CREATE POLICY "Admins can view all profiles" ON public.profiles FOR ALL USING (
  EXISTS (SELECT 1 FROM public.profiles WHERE id = auth.uid() AND (role = 'ceo' OR role = 'admin'))
);
CREATE POLICY "Users can manage own sessions" ON public.sessions FOR ALL USING (auth.uid() = user_id);
CREATE POLICY "Admins can view all sessions" ON public.sessions FOR SELECT USING (
  EXISTS (SELECT 1 FROM public.profiles WHERE id = auth.uid() AND (role = 'ceo' OR role = 'admin'))
);


2. GitHub Deployment

Create a new repository on GitHub.

Upload the adflow_pro.html file and rename it to index.html.

Commit and push your changes.

3. Netlify Hosting

Connect your GitHub account to Netlify.

Select the adflow-pro repository.

Click Deploy.

Once live, open your URL and enter your Supabase Project URL and Anon Key in the setup screen.

🔐 Admin Configuration

To gain Admin/CEO access for the first time:

Register a new account on your deployed site.

Go to your Supabase Dashboard -> Table Editor -> profiles.

Locate your user record and manually change status to active and role to ceo.

Refresh the app to see the Admin Dashboard.

📝 License

Distributed under the MIT License. See LICENSE for more information.
