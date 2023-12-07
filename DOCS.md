# Problem In React JS:
React renders the component on the client side - CSR(Client Side Rendering). Because of it, the seo bots like google bots couldn't recognise the dynamic content in the web page. The dynamic data that is rendered will not be presented on the website when these bots crawls the website. So bots will not have the full content of the site. Because of this, React apps will not have full SEO Support. 

# Why Next JS:
This is the main drawback that next.js fullfills with Concepts like SSR(SERVER SIDE RENDERING), ISG(Incremental static Generation), SSG(Static Site Generation). Because of these features the app built with Next.JS Will comes built in with better SEO and performance.

# Routing in Next JS:
Routing In Next JS is a File System Based. Generally there are two types of routing systems that are available in Next JS:

1. Page Routing(Old Routing System before Next JS 13)
2. App Routing(The Recommended Way of routing)

App routing will be done based on the file system folder structure in "app" directory.
For Ex:
Lets say we wanna have "http://localhost:3000/contact"("/contact") endpoint. we need to create a file syatem as below:
1. Create a folder named "contact" in "app" directory.
2. Create a file named page.tsx within the "contact" folder.
app directory -> contact directory -> page.ts
(here page.ts is the special file that next js looks to register the route)

## Dynamic Routes in Next JS(Route Params in Next JS):
Let's say we wanna create an endpoint like this:
With Params: /posts/:id

We need to create the following folder structure with [] braces as shown below
app directory -> posts directory -> "[id]" folder -> page.tsx

Then we can capture the params passed to the component as params object.

in page.tsx ->

const Postdetail = ({params}: {params: {id: string}}) => {
     // You can access like this -> params.id 
    /posts/1234567 -> params.id -> Fetch post with id 1234567
}

With Search Params:
/posts?page=react

const Props {
    params: {id: string},
    searchParams: {search:  {
        page: string
    }}
}

const Postdetail = ({params, searchParams}: Props) => {
     // You can access like this -> params.id , searchParams.page
    /posts/1234567 -> params.id -> Fetch post with id 1234567
}
# Programatic Routes:
import { useRouter, useSearchParams } from "next/navigation";

const router = useRouter();
const searchParams = useSearchParams();

const changePage = (page: number) => {
    const params = new URLSearchParams(searchParams);
    params.set("page", page.toString());

    // router.push("?" + params.toString());
    // router.push("/issues")
};

Catch All:

Route Example               URL	            params
pages/shop/[...slug]	    /shop/a	        { slug: ['a'] }
pages/shop/[...slug]	    /shop/a/b	    { slug: ['a', 'b'] }
pages/shop/[...slug]    	/shop/a/b/c	    { slug: ['a', 'b', 'c'] }

Optional Catch All:


Route Example               URL	        params
pages/shop/[[...slug]]	/shop	    { slug: [] }
pages/shop/[[...slug]]	/shop/a	    { slug: ['a'] }
pages/shop/[[...slug]]	/shop/a/b	{ slug: ['a', 'b'] }
pages/shop/[[...slug]]	/shop/a/b/c	{ slug: ['a', 'b', 'c'] }


Route Groups:
In the app directory, nested folders are normally mapped to URL Paths. However you can mark a folder as a Route Group to prevent the folder from being included in the routes URL.

All you need to do is wrap the folder name in paranthesis - "(auth)". So that your directory structure in your app cannot be revealed to end user.

For Example:
app -> (auth) -> sign-in -> page.tsx => /sign-in
app -> (auth) -> sign-up -> page.tsx => /sign-up

# Client vs Server Paradigm:
client -> your Browser
server -> Your Server

In Next JS, Components can be either Client Component or Server Component.
Both are react component. But the difference is where they rendered.

A server components can reduce initial page load by recieving js bundle of less size. All the rendering(Creation of final HTML file to render) will be taken care in server side. This will enhance SEO

# When to use Client Component or Server Component:

Usecases to make Server Componenets:
1. Fetching Data
2. Access Backend Resources
3. Keep Sensitive info on server(like tokens)
4. Keep large dependencies on server(Reduce Client side Javascript)

Usecases to make Client Components: 
1. Add Interactivity and event listeners(onClick(), onChange())
2. Use State and Lifecycle effects - useState(), useEffect() etc.,
3. Use browser only API's
4. Use Custome hooks that depends on state, effects or browser only api's
5. React Class Components

In Next Js 13+, By default all the components that are inside app directory are Server Components.

To make a client component just use "use client" directive at the top of the component Page.

"use client"
// your imports
const example = () => {
    // Your Component
}

Note: A server component nested inside a client component is always treated as a client component, So, donot include a server components in a client component.

# Different Rendering Strategies
In Next JS 13+, We have Strategies(Different ways that things are displayed) and RunTime/BuildTime(the specific time they run) and Environment(the specific space they work).

Rendering -> It's a process of generating or creating the user interface(final HTML) from the code we write.

Rendering Process can occure in client side or on server side before the final HTML is being sent to client.
CSR(Client SIde Rendering) -> React -> Can be done in B2B(business-to-business) domain apps. 
SSR(Server Side rendering) -> Next JS -> Can be done when we need more SEO for the app.

# The Time
Once the High level Programming language gets converted to Binary codes(Completion of Compilation), Our app goes through 2 phases: 1) Build Time, 2) Run Time.

Build Time -> Seriues of steps where we(the developer) prepare code for production. Like "npm run build" and "npm run dev"

Run Time -> It refers when the deployed app is actively executing or running. i.e., when our client runs a piece of code. Like User Interactions etc.,

# Run Time Environment: It is an environment
Like Node JS, Which lets us run js in a computer or machine.
Next Js Supports 2 different run time environments to execute our applications.
1. The Node JS Run time(Has access to all Node API's and ecosystem)
2. The Edge Run Time(A Light weight based on Web APi's and contains subset of Node JS API's)

By default next js uses nodejs as run time. we can change this to edge by

export const runtime = "edge"; ("nodejs" is default)

# Rendering Strategies:
Depending on the above rendering enviroment and time period(build/run time), Next Js Supports 3 Strategies.

1. Static SIte Generation(SSG): Happens at build time on Server to generate static pages(Final HTML, CSS & JS) of our app. These pages doesnt require any interaction with the server at run time. This content is cached and reused on subsequent requests. Since it runs at build time, we need to rebuild our app if we change the content of these pages. To overcome this limitation Next Js introduced Incremental Static Generation.
2. Incremental Static Generation(ISG): It allows us to update these static pages after we build them without needing to rebuild again the entire site. i., certain parts of our website(few pages) are generated at build time while others are generated on runtime by user activity. This is like a hybrid approach. This will improves the overall performance of the website by updating only certain requested pages of regeneration.
3. Server Side rendering(SSR): Dynamic rendering. generates the dynamic content of each request on server and sends it(final HTML, CSS, JS) back to client. SSR excels in situations where a website heavily dependent on client side updates and requires real time updates like data for dynamic realtime charts in a dashboard.

if(Page displays same info on each request) {
    // use SSG
} else if(Page requires frequent info updates) {
    // use SSR
} else {
    // use ISG
}

# Server Actions
Next.js Server Actions are a new feature introduced in next js 13 that allows you to run server code without having to create an API endpoint. A server action is a function that runs on a server, But we can call it from the client just like normal functions.

## What can we do with server actions
1. Write into a db
2. Write server logic
3. calling external apis

## Pros of server actions
1. No need to create an API end point
2. Less Code
3. Jumping into definition(No need to search for server side edefinitions)
4. Type Safety

## How to define a Next Js Server Action
Before you start, ensure you have enabled the experimental server actions in your next.config.js file:


module.exports = {
  experimental: {
    serverActions: true,
  }
};

Server actions can be defined any where in your component but with a few exceptions.

Create an async functions and use "use server" declerative at the top of the function body.

async function myActionFunction() {
    "use server";

    // do something like interacting with db
}

## Defining Multiple Next.js Server Actions
Export multiple functions from a single file  adding "use server" at the top.

test.js
---------------------
"use server";

export async function myActionFunction() {

    // do something like interacting with db
}

export async function myActionFunctionTwo() {

    // do something like interacting with db
}

## Invoking Server Actions

export function MyFormComponent() {
  function handleFormAction(
    formData: FormData
  ) {
    'use server';
 
    const name = formData.get('name');
    // do something
  }
 
  return (
    <form action={handleFormAction}>
      <input type={'name'} />
 
      <button type="submit">Save</button>
    </form>
  );
}


  