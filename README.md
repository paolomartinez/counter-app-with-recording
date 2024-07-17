# Counter App with Screen Recording

## Table of Contents
1. [Project Overview](#project-overview)
2. [Prerequisites](#prerequisites)
3. [Setup Instructions](#setup-instructions)
4. [Project Structure](#project-structure)
5. [Key Components](#key-components)
6. [Web APIs Used](#web-apis-used)
7. [Running the Application](#running-the-application)
8. [Troubleshooting](#troubleshooting)
9. [Security Considerations](#security-considerations)
10. [Browser Compatibility](#browser-compatibility)

## 1. Project Overview

This project is a React-based web application that creates a customizable counter with screen recording capabilities. The app allows users to start a screen recording, increment a counter using the spacebar, and then stop and export the recording as an MP4 file.

## 2. Prerequisites

Before setting up the project, ensure you have the following installed:

- Node.js (version 14.0.0 or later)
- npm (usually comes with Node.js)
- A modern web browser (Chrome, Firefox, or Edge recommended for full compatibility)

## 3. Setup Instructions

1. Create a new React project:
   ```
   npx create-react-app counter-app-with-recording
   cd counter-app-with-recording
   ```

2. Install necessary dependencies:
   ```
   npm install @radix-ui/react-slot lucide-react class-variance-authority clsx tailwind-merge
   npm install -D tailwindcss postcss autoprefixer
   ```

3. Initialize Tailwind CSS:
   ```
   npx tailwindcss init -p
   ```

4. Replace the content of `tailwind.config.js` with:
   ```javascript
   /** @type {import('tailwindcss').Config} */
   module.exports = {
     content: [
       "./src/**/*.{js,jsx,ts,tsx}",
     ],
     theme: {
       extend: {},
     },
     plugins: [],
   }
   ```

5. Add Tailwind directives to your `./src/index.css`:
   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

6. Create a new file `./src/components/ui/button.jsx`:
   ```jsx
   import * as React from "react"
   import { Slot } from "@radix-ui/react-slot"
   import { cva } from "class-variance-authority";
   import { cn } from "../../lib/utils"

   const buttonVariants = cva(
     "inline-flex items-center justify-center rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:opacity-50 disabled:pointer-events-none ring-offset-background",
     {
       variants: {
         variant: {
           default: "bg-primary text-primary-foreground hover:bg-primary/90",
           destructive:
             "bg-destructive text-destructive-foreground hover:bg-destructive/90",
           outline:
             "border border-input hover:bg-accent hover:text-accent-foreground",
           secondary:
             "bg-secondary text-secondary-foreground hover:bg-secondary/80",
           ghost: "hover:bg-accent hover:text-accent-foreground",
           link: "underline-offset-4 hover:underline text-primary",
         },
         size: {
           default: "h-10 py-2 px-4",
           sm: "h-9 px-3 rounded-md",
           lg: "h-11 px-8 rounded-md",
         },
       },
       defaultVariants: {
         variant: "default",
         size: "default",
       },
     }
   )

   const Button = React.forwardRef(({ className, variant, size, asChild = false, ...props }, ref) => {
     const Comp = asChild ? Slot : "button"
     return (
       <Comp
         className={cn(buttonVariants({ variant, size, className }))}
         ref={ref}
         {...props}
       />
     )
   })
   Button.displayName = "Button"

   export { Button, buttonVariants }
   ```

7. Create a new file `./src/components/ui/input.jsx`:
   ```jsx
   import * as React from "react"
   import { cn } from "../../lib/utils"

   const Input = React.forwardRef(({ className, type, ...props }, ref) => {
     return (
       <input
         type={type}
         className={cn(
           "flex h-10 w-full rounded-md border border-input bg-background px-3 py-2 text-sm ring-offset-background file:border-0 file:bg-transparent file:text-sm file:font-medium placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50",
           className
         )}
         ref={ref}
         {...props}
       />
     )
   })
   Input.displayName = "Input"

   export { Input }
   ```

8. Create a new file `./src/lib/utils.js`:
   ```javascript
   import { clsx } from "clsx"
   import { twMerge } from "tailwind-merge"

   export function cn(...inputs) {
     return twMerge(clsx(inputs))
   }
   ```

9. Replace the content of `./src/App.js` with the Counter App component code.

## 4. Project Structure

After setup, your project structure should look like this:

```
counter-app-with-recording/
├── node_modules/
├── public/
├── src/
│   ├── components/
│   │   └── ui/
│   │       ├── button.jsx
│   │       └── input.jsx
│   ├── lib/
│   │   └── utils.js
│   ├── App.js
│   ├── index.js
│   └── index.css
├── package.json
├── tailwind.config.js
└── postcss.config.js
```

## 5. Key Components

- `App.js`: The main component containing the counter and screen recording logic.
- `button.jsx`: A reusable button component from shadcn/ui.
- `input.jsx`: A reusable input component from shadcn/ui.

## 6. Web APIs Used

This project utilizes the following Web APIs:

1. `navigator.mediaDevices.getDisplayMedia()`: Used to capture the screen for recording.
2. `MediaRecorder`: Used to record the screen capture stream.
3. `URL.createObjectURL()`: Used to create a URL for the recorded video blob.

## 7. Running the Application

To run the application:

1. Navigate to the project directory in your terminal.
2. Run `npm start`.
3. Open your browser and go to `http://localhost:3000`.

## 8. Troubleshooting

- If you encounter issues with screen recording, ensure you're using a compatible browser (Chrome, Firefox, or Edge).
- Make sure you've granted the necessary permissions for screen recording when prompted by the browser.
- If the app doesn't work as expected, check the browser's console for any error messages.
- If the exported MP4 file doesn't play correctly, try opening it with VLC Media Player or converting it using a local video converter application.

## 9. Security Considerations

- Screen recording is a sensitive operation. Ensure your application is served over HTTPS in production.
- Be aware that `getDisplayMedia()` will prompt the user to select which screen or application to share. The user must actively choose to share their screen.
- Handle the recorded data securely. In this example, the data is kept in memory and offered for download, but in a production app, you might want to implement secure storage and transmission of the recorded data.

## 10. Browser Compatibility

The Screen Capture API is supported in:
- Chrome 72+
- Firefox 66+
- Edge 79+
- Opera 60+

Note that Safari and Internet Explorer do not support this API.

For the most up-to-date compatibility information, check [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/API/Screen_Capture_API/Using_Screen_Capture).

Keep in mind that while we're attempting to record directly to MP4, not all browsers support this. In cases where MP4 recording is not supported, the app will fall back to WebM recording but still export with an .mp4 extension. This may cause compatibility issues with some media players.
