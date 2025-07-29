# AI-Ready-Apps
The detailed list of instructions to make a production ready application by just listing a few prompts

First Create an account for using Cursor

Step # 1 [First Install Node in your Computer]
Note: Please install Node JS and check node, npm and npx (On left side, you will create a folder)

Install Shadcn using command

```
npx shadcn@latest init
```

Step # 2
Jump into the next JS folder instead of the root directory, Cursor will always do issues when you are not in right directory.
Go onto the top, Write
```
>Install cursor command (add your password)

Inside the terminal write:
cursor my-app (Your Project Name)
```

Note: The advantage of above step is that you can use "npm run dev" to start the project server from packages JSON file. Check by Opening your 'http://localhost:3000'


Step # 3
Shadcn don't add all components by itself. Just go to terminal, Click on + Button, Open new Terminal.
```
npx shadcn@latest add
```

Select button, card and input from the space bar. You will automatically see the components folder created


Step # 4
In your folder make a new folder named as "requirements" and Inside the folder, Create a markdown file named as 'frontend_instructions.md'
Note 1: for Relevant Docs we have to visit this link and copy the contents in API Tab.
Note 2: Take a screenshot from left and ask chat "To generate a file structure in ASCII version"
```
# Project Overview

Use this guide to build a web app where user can give a text prompt to generate emoji using a model hosted on replicate

# Feature requirements

- We will use Next.Js, shadcn, lucid, Supabase and Clerk
- Create a form where users can put in a prompt and clicking on button that calls a replicate model to generate emoji
- Have a nice UI and animation when emoji is blank or generating
- Display all the images ever generated in grid
- When hover each emoj img, an icon button for download, and an icon button for like should be shown up

# Relevant docs
## How to use replicate emoji generator model

Set the REPLICATE_API_TOKEN environment variable

export REPLICATE_API_TOKEN=<paste-your-token-here>


Install Replicate’s Node.js client library

npm install replicate

Run fofr/sdxl-emoji using Replicate’s API. Check out the model's schema for an overview of inputs and outputs.

import { writeFile } from "fs/promises";
import Replicate from "replicate";
const replicate = new Replicate();

const input = {
    prompt: "A TOK emoji of a man",
    apply_watermark: false
};

const output = await replicate.run("fofr/sdxl-emoji:dee76b5afde21b0f01ed7925f0665b7e879c50ee718c5f78a9d38e04d523cc5e", { input });

// To access the file URLs:
console.log(output[0].url());
//=> "https://replicate.delivery/.../output_0.png"

// To write the files to disk:
for (const [index, item] of Object.entries(output)) {
  await writeFile(`output_${index}.png`, item);
}
//=> output_0.png written to disk


xxx

# Current File Structure
MY-APP/
├── .next/                    # Next.js build output (collapsed)
├── app/                      # Next.js app directory (expanded)
│   ├── favicon.ico          # Favicon file
│   ├── globals.css          # Global styles (modified)
│   ├── layout.tsx           # Root layout component
│   └── page.tsx             # Home page component
├── components/               # UI components (expanded)
│   └── ui/                  # UI component library
│       ├── button.tsx       # Button component (untracked)
│       ├── card.tsx         # Card component (untracked)
│       └── input.tsx        # Input component (untracked)
├── lib/                      # Utility libraries (expanded)
│   └── utils.ts             # Utility functions (untracked)
├── node_modules/             # Dependencies (collapsed)
├── public/                   # Static assets (collapsed)
├── requirements/             # Project requirements (expanded, selected)
│   └── frontend_instructions.md  # Project documentation (untracked, selected)
├── .gitignore               # Git ignore rules
├── components.json           # shadcn/ui configuration (untracked)
├── eslint.config.mjs        # ESLint configuration
├── next-env.d.ts           # Next.js TypeScript definitions
├── next.config.ts           # Next.js configuration
├── package-lock.json        # Dependencies lock file (modified)
├── package.json             # Project dependencies (modified)
├── postcss.config.mjs       # PostCSS configuration
├── README.md                # Project documentation
└── tsconfig.json           # TypeScript configuration
```

# Rules
- All new components should go in /components folder like example-component.tsx unless otherwise specified
- All new pages should go in /app

Step # 5
Install Replicate in Terminal
```
npm install replicate
```

Create a new file in project folder ".env.local" and add:
REPLICATE_API_TOKEN=xxxxxxxxxxxxxxxxxxxxxx


Step # 6
First Add your mockup.png in requirements folder and proceed.
Click 'command + I' on Mac, and Select Page.tsx file and write in Box:

```
Help me build the whole emoji maker app based on @frontend_instructions.md and UI looks similar to @mockups.png
```


Step # 7 
Solve errors as they occur sequentially

Step # 8
The functionality of generating emoji is working, however the newly generated emoji should be added to the image grid.
What would be the steps to achieve this functionality, Let's think step by step.

Step # 9
NOTE: Select the Page.tsx in cursor
Now lets create a feature for download image and like image
1. Clicking on download image button, it should download the image to local computer
2. Click on like icon button it should auto turn the button to filled style to indicate the image has been liked by this user already; and also increment count of no of likes


Step # 10
Go to Clerk, Make an Auth by Drag and Drop (Just Follow the Steps on WebPage
Coppy the middleware.ts code and add it in Root folder

Rewrite middleware.ts code:

```
import { clerkMiddleware, createRouteMatcher } from "@clerk/nextjs/server";
import { NextResponse } from "next/server";

const isProtectedRoute = createRouteMatcher(["/"]);

export default clerkMiddleware(async (auth, req) => {
  const { userId, redirectToSignIn } = auth();

  // If the user isn't signed in and the route is private, redirect to sign-in
  if (!userId && isProtectedRoute(req)) {
    return redirectToSignIn({ returnBackUrl: "/" });
  }

  // If the user is logged in and the route is protected, let them view
  if (userId && isProtectedRoute(req)) {
    return NextResponse.next();
  }
});

export const config = {
  matcher: ["/((?!.*\\..*|_next).*)", "/", "/(api|trpc)(.*)"],
};

```


Step # 11
Write headers.tsx file and paste this code:

```
"use client";

import { Button } from "@/components/ui/button";
import { SignedIn, SignedOut, SignInButton, UserButton } from "@clerk/nextjs";
import { Menu, X } from "lucide-react";
import { useState } from "react";

export default function Header() {
  const [isMenuOpen, setIsMenuOpen] = useState(false);

  const toggleMenu = () => {
    setIsMenuOpen(!isMenuOpen);
  };

  return (
    <div className="flex items-center space-x-4 justify-end mt-4 mr-4">
      <SignedOut>
        <SignInButton />
      </SignedOut>
      <SignedIn>
        <UserButton />
      </SignedIn>

      <div className="md:hidden">
        <Button
          variant="ghost"
          size="icon"
          onClick={toggleMenu}
          aria-label="Toggle menu"
        >
          {isMenuOpen ? <X className="h-6 w-6" /> : <Menu className="h-6 w-6" />}
        </Button>
      </div>
    </div>
  );
}
```


Add the Clerk provide in layout.tsx file as well.

```
import type { Metadata } from "next";
import localFont from "next/font/local";
import "./globals.css";
import { ClerkProvider } from "@clerk/nextjs";
import Header from "@/components/headers";

const geistSans = localFont({
  src: "./fonts/GeistVF.woff",
  variable: "--font-geist-sans",
  weight: "100 900",
});

const geistMono = localFont({
  src: "./fonts/GeistMonoVF.woff",
  variable: "--font-geist-mono",
  weight: "100 900",
});

export const metadata: Metadata = {
  title: "Create Next App",
  description: "Generated by create next app",
};

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <ClerkProvider>
      <html lang="en">
        <body
          className={`${geistSans.variable} ${geistMono.variable} antialiased`}
        >
          {children}
        </body>
      </html>
    </ClerkProvider>
  );
}
```

- Go to Supabase, Create a new project, Click Connect (App framework): Copy the credetials into your .env.local file

Creation of Table in Supabase:
```
CREATE TABLE profiles (
  user_id TEXT PRIMARY KEY,
  credits INTEGER DEFAULT 3 NOT NULL,
  tier TEXT NOT NULL DEFAULT 'free' CHECK (tier IN ('free', 'pro')),
  stripe_customer_id TEXT,
  stripe_subscription_id TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP NOT NULL,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP NOT NULL
);
```


Creation of Tables in Supabase Part-2:

```
CREATE TABLE emojis (
  id BIGINT PRIMARY KEY GENERATED BY DEFAULT AS IDENTITY,
  image_url TEXT NOT NULL,
  prompt TEXT NOT NULL,
  likes_count NUMERIC DEFAULT 0,
  creator_user_id TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```


Step # 12
Make a md file named as backend requirements:

```
Project overview
Use this guide to build backend for the web app of emoji maker

Tech stack
We will use Next.js, Supabase, Clerk

Tables & buckets already created
Supabase storage Bucket: emojis

TABLE: emojis
pgsql
Copy
Edit
id BIGINT PRIMARY KEY GENERATED BY DEFAULT AS IDENTITY,  
image_url TEXT NOT NULL,  
prompt TEXT NOT NULL,  
likes_count NUMERIC DEFAULT 0,  
creator_user_id TEXT NOT NULL,  
created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
TABLE: profiles
pgsql
Copy
Edit
user_id TEXT PRIMARY KEY,  
credits INTEGER DEFAULT 3 NOT NULL,  
tier TEXT NOT NULL DEFAULT 'free' CHECK (tier IN ('free', 'pro')),  
stripe_customer_id TEXT,  
stripe_subscription_id TEXT,  
created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP NOT NULL,  
updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP NOT NULL
Requirements
1. Create user to user table

After a user signs in via Clerk, we should get the userId from Clerk, and check if this userId exists in profiles table, matching user_id.

If the user doesn't exist, then create a user in profiles table.

If the user exists already, then proceed, and pass on user_id to functions like generateEmojis.

2. Upload emoji to emojis Supabase storage bucket

When user generates an emoji, upload the emoji image file returned from Replicate to Supabase emojis storage bucket.

Add the image URL to the emojis data table as image_url, and creator_user_id to be the actual user_id.

3. Display all images in emojiGrid

Emoji grid should fetch and display all images from emojis data table.

When a new emoji is generated, the emojiGrid should be updated automatically to add the new emoji to the grid.

4. Likes interaction

When user checks the 'like' button, the likes_count should increase on the emojis table.

When user un-checks the 'like' button, the likes_count should decrease on the emojis table.

Documentations
Example of uploading files to Supabase storage
js
Copy
Edit
import { createClient } from '@supabase/supabase-js'

// Create Supabase client
const supabase = createClient('your_project_url', 'your_supabase_api_key')

// Upload file using standard upload
async function uploadFile(file) {
  const { data, error } = await supabase.storage
    .from('bucket_name')
    .upload('file_path', file)

  if (error) {
    // Handle error
  } else {
    // Handle success
  }
}

```

Step # 13
Select layout.tsx file and then Prompt:

I've built an emoji maker web app frontend based on @frontend_instructions.md, now help me build the backend feature based on @backend_instructions.md;










