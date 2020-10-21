# Building Distributed High Availability Web Apps with IPFS

## IPFS Crash Course Presentation

* [Slideshow](/)

## Deploying a Web Application to the IPFS Network

For this activity we will be building and deploying a NextJS based web application to IPFS.

### Installing NextJS

Before we can begin our basic web application we need to start by installing the NextJS javascript library from npm. We can do this by opening a new terminal and typing:

```bash
npm i next -g
```

* NextJS is a javascript framework for web applications, let's take a look their website [https://nextjs.org/](https://nextjs.org/)

* The reason we are using NextJS for this activity is because:

  * It drastically reduces the development time of web applications.

  * It supports hybrid static & server rendering which is perfect for serving static without the need of nodeJS or some other server, this allows our application to be served from any backing data store that is able to serve static files.

### Creating a New NextJS Project

After installation we can create a new NextJS project by running.

```bash
npx create-next-app trilliondollarapp
```

For the static type lovers:

```bash
npx create-next-app --example with-typescript trilliondollarapp
```

* We will be prompted for a new project name.

* As you can see it's automatically generating a new project and installing react, react-dom, and next using npm.

When the new NextJs project has been generated successfully you will be greeted by a success screen with the location of the new project a list of commands for serving the current build in either development or production mode.

Let's take a look at our new project. We can `cd` into our new project with:

```bash
cd trilliondollarapp
```

Now let's take a look at our NextJS project's generated `package.json`.

```bash
cat package.json
```

This gives us:

```json
{
  "name": "trilliondollarapp",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "9.5.5",
    "react": "17.0.0",
    "react-dom": "17.0.0"
  }
}
```

* This is pretty bare bones.

* We just need to add one script, called `export`, that allows us to build a static folder for our site.

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start",
  "export": "next export"
}
```

One more thing, we'll need to configure Next.js to use relative paths when uploading to IPFS, since we can't guarantee we'll be at the root of a domain.bash

We'll need to create a `next.config.js` and insert the following:

```javascript
module.exports = { assetPrefix: './' }
```

Or use a Bash one-liner:

```bash
echo "module.exports = { assetPrefix: './' }" > next.config.js
```

Let's see what the folder structure and code for our new project looks like.

```bash
.
├── .next
├── pages
│   └── api
├── public
└── styles
```

* **.next** - the .next folder contains the current static build and a cache for webpack and babel.

* **node_modules** - node_modules is all of our projects npm dependencies.

* **pages** - we can create any React components pages in our pages folder and NextJS will automatically generate routes for them.

  * As a bonus NextJS also has API routes which makes creating new APIs incredibly simple, but we won't be using that in this demonstration.

* **public** - The good ole classic public folder, anything we put in this folder will be served normally from any static webserver.

* **styles** - A place for all of our css stylesheets.

Cool, now let's run our project in development mode. This will provide awesome features like hot reloading. We can run our project with:

```bash
npm run dev
```

* By default this will launch our dev server at [http://localhost:3000](http://localhost:3000)

We can view the code of our projects main index page by opening `pages/index.js`. Let's modify this code a bit for our demo.

```javascript
    <div className={styles.container}>
      <Head>
        <title>IEEE Cloud Demo</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <main className={styles.main}>
        <h1 className={styles.title}>
          Hello <a href="https://github.com/akashictech/ieee-2020-ipfs">IEEE Cloud Summit 2020</a>
        </h1>

        <p className={styles.description}>
          Find this tutorial on github!
          <a href="https://github.com/akashictech/ieee-2020-ipfs">
          <code className={styles.code}>akashictech/ieee-2020-ipfs</code></a>
        </p>
      </main>
    </div>
```

* React is pretty much vanilla javascript these days.

* A React component is a javascript function that returns a piece of html code. There are minor differences such as using `className` instead of `class` to make it compatible syntax without conflicting with native keywords.

Now that we have a basic web application that we can serve, let's build it and deploy it to IPFS. We can build our NextJS project with:

```bash
npm run build
npm run export
```

This will create an `out` folder that contains our frontend.

Now let's use a popular IPFS pinning service to **Pinata**. Pinata hosts a global network of IPFS nodes to back files on the IPFS network. We can access Pinata by navigating to [https://pinata.cloud](https://pinata.cloud).

Pinata runs a simple web upload tool that allows developers to easily pin a directory on the IPFS network.

* Let's drag and drop our `out` folder and upload it.

This will give us an IPFS hash like `QmP7RwEYnV72tPXd2fLQKHtxeYKLd3Zd23143pisW3PZzM`.

We can then open our site in any IPFS gateway. Cloudflare, IPFS.io, Infura, and Pinata are all popular options. You can host one yourself easily by running your own node as well! We'll get to that in a bit.

* [ipfs.io/ipfs/QmP7RwEYnV72tPXd2fLQKHtxeYKLd3Zd23143pisW3PZzM](https://ipfs.io/ipfs/QmP7RwEYnV72tPXd2fLQKHtxeYKLd3Zd23143pisW3PZzM)

* [cloudflare-ipfs.com/ipfs/QmP7RwEYnV72tPXd2fLQKHtxeYKLd3Zd23143pisW3PZzM](https://cloudflare-ipfs.com/ipfs/QmP7RwEYnV72tPXd2fLQKHtxeYKLd3Zd23143pisW3PZzM)

* [infura.io/ipfs/QmP7RwEYnV72tPXd2fLQKHtxeYKLd3Zd23143pisW3PZzM](https://ipfs.io/ipfs/QmP7RwEYnV72tPXd2fLQKHtxeYKLd3Zd23143pisW3PZzM)

* [gateway.pinata.cloud/ipfs/QmP7RwEYnV72tPXd2fLQKHtxeYKLd3Zd23143pisW3PZzM](https://gateway.pinata.cloud/ipfs/QmP7RwEYnV72tPXd2fLQKHtxeYKLd3Zd23143pisW3PZzM)
