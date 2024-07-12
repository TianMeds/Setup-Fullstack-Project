# Full Stack Next.js Application Setup

This guide will walk you through setting up a full-stack Next.js application with Node.js, Visual Studio Code, Postman for testing, Shadcn/UI for components, and Clerk for authentication.

## Prerequisites

Before you begin, ensure you have the following installed on your machine:

- [Node.js](https://nodejs.org/)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Postman](https://www.postman.com/)

## Steps

### 1. Create a New Next.js Project

1. Create a folder in your desired location.
2. Open a terminal in the folder and run the following command:

    ```bash
    npx create-next-app@latest
    ```

3. You will be prompted with the following options. For this guide, select "yes" for Tailwind CSS and App Router.

    ```
    What is your project named? my-app
    Would you like to use TypeScript? No
    Would you like to use ESLint? Yes
    Would you like to use Tailwind CSS? Yes
    Would you like to use `src/` directory? No
    Would you like to use App Router? (recommended) Yes
    Would you like to customize the default import alias (@/*)? No
    ```

### 2. Open the Project in Visual Studio Code

1. Open Visual Studio Code.
2. Open the folder you created and navigate to `app/page.js`.
3. Replace the default HTML code with your desired content.

### 3. Run the Development Server

1. In the terminal, run:

    ```bash
    npm run dev
    ```

2. Your application will be available at `http://localhost:3000`.

### 4. Install Shadcn/UI for Components

1. Run the following command to install Shadcn/UI:

    ```bash
    npx shadcn-ui@latest init
    ```

2. Select the following options:

    ```
    Which style would you like to use? › Default
    Which color would you like to use as base color? › Neutral
    Do you want to use CSS variables for colors? › Yes
    ```

3. Refer to the [Shadcn/UI documentation](https://ui.shadcn.com/docs/components/) for further details on using components.

### 5. Set Up Authentication with Clerk

1. Sign up at [Clerk](https://clerk.com/).
2. Create a new application in the Clerk dashboard and configure social media login options.
3. Install Clerk in your project:

    ```bash
    npm install @clerk/nextjs
    ```

4. Copy the `.env` variables provided by Clerk and create a `.env.local` file in your project.

5. Create a `middleware.js` file in your root folder and add the following code:

    ```javascript
    import {
      clerkMiddleware,
      createRouteMatcher
    } from '@clerk/nextjs/server';

    const isProtectedRoute = createRouteMatcher([
      '/dashboard(.*)',
      '/forum(.*)',
    ]);

    export default clerkMiddleware((auth, req) => {
      if (isProtectedRoute(req)) auth().protect();
    });

    export const config = {
      matcher: ['/((?!.*\\..*|_next).*)', '/', '/(api|trpc)(.*)'],
    };
    ```

6. Update `layout.js` to include `ClerkProvider`:

    ```javascript
    import { ClerkProvider } from "@clerk/nextjs";

    <ClerkProvider>
      <html lang="en">
        <body className={inter.className}>{children}</body>
      </html>
    </ClerkProvider>
    ```

7. Protect routes by following the [Clerk middleware documentation](https://clerk.com/docs/references/nextjs/clerk-middleware#clerk-middleware).

### 6. Create Protected Routes

1. Create a `dashboard` folder in the `app` directory with `layout.jsx` and `page.jsx`:

    **layout.jsx**
    ```javascript
    import React from 'react';

    const DashboardLayout = ({ children }) => {
      return <div>{children}</div>;
    };

    export default DashboardLayout;
    ```

    **page.jsx**
    ```javascript
    import React from 'react';

    const Dashboard = () => {
      return <div>Dashboard</div>;
    };

    export default Dashboard;
    ```

2. Test the route by navigating to `http://localhost:3000/dashboard`. It should redirect to the sign-up page if not logged in.

### 7. Customize Sign-In and Sign-Up Pages

1. Create a folder named `auth` inside the `app` directory.
2. Create `sign-up/[[...sign-up]]/page.jsx` with the following code:

    ```javascript
    import { SignUp } from "@clerk/nextjs";

    export default function Page() {
      return <SignUp />;
    }
    ```

3. Create `sign-in/[[...sign-in]]/page.jsx` with the following code:

    ```javascript
    import { SignIn } from "@clerk/nextjs";

    export default function Page() {
      return <SignIn />;
    }
    ```

4. Update `.env.local`:

    ```env
    NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in
    NEXT_PUBLIC_CLERK_SIGN_UP_URL=/sign-up
    ```

5. Restart the development server:

    ```bash
    npm run dev
    ```

Now your application should have authentication with Clerk and be running on `http://localhost:3000`.

### Useful Links

- [Clerk Elements](https://clerk.com/docs/elements/overview)
- [Hyper UI](https://www.hyperui.dev/)
