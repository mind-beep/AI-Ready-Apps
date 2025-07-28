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












