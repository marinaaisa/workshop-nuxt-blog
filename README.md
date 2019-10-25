<p align="center">
  <a href="https://nuxtjs.org/">
  <img src="https://avatars2.githubusercontent.com/u/23360933?s=200&v=4" height="60">
  </a>
  +
  <img src="https://geekytheory.com/wp-content/uploads/2014/03/markdown_inte-1024x630.png" height="60">
</p>
<h1 align="center">
  Building a multilingual blog using Vue.js, Nuxt and Markdown
</h1>

## ⚡️ Live
[Check it live](https://nuxt-markdown-blog-starter.netlify.com/)

## Step 0

0.  **You need to install Node first in your computer**

    **For Mac:**

    Install Homebrew (if you don't have it yet)...

    ```sh
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```
    ... and then install Node through Homebrew:

    ```sh
    brew install node
    ```

    **For Windows** you can download it [here](https://nodejs.org/es/download/).

## ✨ Step 1: Download the project and get started

1.  **Clone this repository.**

    ```sh
    git clone https://github.com/marinaaisa/workshop-nuxt-blog.git
    ```

2.  **Go to the project and install npm.**

    Navigate into your new site’s directory and install all the needed node packages.

    ```sh
    cd workshop-nuxt-blog/
    npm install
    git fetch
    ```

3.  **Go to branch called Step 1.**

    ```sh
    git checkout step-1
    ```

4.  **Running!**

    ```sh
    npm run dev
    ```

    Your site is now running at `http://localhost:3000`!

## Step 2: Create your first Markdown file in English

1.  **Go to `contents/en/blog/` to create your blog post in Markdown (.md) with this syntaxis:**

  Remember the name of this file will be the name of the URL and it has to be the same as the `name` property inside the Markdown
  
  Example:

    ```md
    ---
    name: 'bacon-ipsum'
    title: Bacon Ipsum
    year: 8 November 2019
    id: 'bacon-ipsum'
    description: |
      Bacon ipsum dolor amet spare ribs ham t-bone buffalo prosciutto, frankfurter bresaola short ribs cupim ground round filet mignon shoulder pork chuck strip steak.
    ---

    Bacon ipsum dolor amet spare ribs ham t-bone buffalo prosciutto, frankfurter bresaola short ribs cupim ground round filet mignon shoulder pork chuck strip steak. Jowl biltong meatloaf ham hock alcatra hamburger pork chop andouille pastrami leberkas frankfurter short ribs bacon venison. Shoulder pork belly andouille burgdoggen.
    ```

2. **Go to `contents/en/blogsEn.js` and write the `name` of your blog post inside the exported array.** Example:

  ```js
  export default [
    'bacon-ipsum',
  ]
  ```

## Step 2: Translate it to Spanish

1. Do the same inside `contents/es/blog/` and write a new markdown file with your translated blog post. Remember to translate the name of the file as well. `id` has to be the same as the one in English. Example:

  ```md
      ---
      name: 'jamon-ipsum'
      title: Jamon Ipsum
      year: 8 Noviembre 2019
      id: 'bacon-ipsum'
      description: |
        Jamón ipsum borrachos como cubas flamenco caramba picha. Y los reconquista ronda manchego. Barça y mi de vicio morcilla litros. Tomatito y la ojos al tuntún, tu chorizo gorilla y mucho de peluco ancha es Castilla.
      ---

      Jamón ipsum borrachos como cubas flamenco caramba picha. Y los reconquista ronda manchego. Barça y mi de vicio morcilla litros. Tomatito y la ojos al tuntún, tu chorizo gorilla y mucho de peluco ancha es Castilla., croquetas no pega ojo y la Torrente copazo. Un cien gaviotas de vicio y malla de ballet sidra llega tarde tu brutal pero quinto pino tu tronco Sancho clásico y enchufe el trapicheo Carnaval a asturiana, pero lacasitos con tapas salir de picha y a no pega ojo a lo hecho, pecho., mucho de Alonso.
  ```

2. **Do the same as in English, go to `contents/es/blogsEs.js` and write the `name` of your blog post inside the exported array.** Example:

  ```js
  export default [
    'jamon-ipsum',
  ]
  ```

## Step 3: Add a webpack loader for your Markdown files

1. Install `frontmatter-markdown-loader`

  ```sh
    npm install frontmatter-markdown-loader
    ```

2. Go to `nuxt.config.js` and add inside `config.module.rules.push(` this object:
  ```js
  {
    test: /\.md$/,
    loader: 'frontmatter-markdown-loader',
    include: path.resolve(__dirname, 'contents'),
    options: {
      vue: {
        root: "dynamicMarkdown"
      }
    }
  }
  ```

## Step 4: Import your Markdown files

1. Go to `pages/index` and inside the `asyncData` function add:

  ```js
  const blogs = app.i18n.locale === 'en' ? blogsEn : blogsEs
        
  async function asyncImport (blogName) {
    const wholeMD = await import(`~/contents/${app.i18n.locale}/blog/${blogName}.md`)
    return wholeMD.attributes
  }

  return Promise.all(blogs.map(blog => asyncImport(blog)))
  .then((res) => {
    return {
      blogs: res
    }
  })
  ```

2. Go to `pages/_slug` and inside the `asyncData` function add:

  ```js
  const fileContent = await import(`~/contents/${app.i18n.locale}/blog/${params.slug}.md`)
  const attr = fileContent.attributes
  return {
    name: params.slug,
    title: attr.title,
    trans: attr.trans,
    year: attr.year,
    id: attr.id,
    owner: attr.owner,
    colors: attr.colors,
    role: attr.role,
    cardAlt: attr.cardAlt,
    noMainImage: attr.noMainImage,
    description: attr.description,
    related: attr.related,
    extraComponent: attr.extraComponent,
    renderFunc: fileContent.vue.render,
    staticRenderFuncs: fileContent.vue.staticRenderFns,
    image: {
      main: attr.image && attr.image.main,
      og: attr.image && attr.image.og
    }
  }
  ```

## Step 4: Add transitions


## Step 5: Deploy on Netlify

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/marinaaisa/nuxt-markdown-blog-starter)

## 🧐 What's inside?

    .
    ├── node_modules
    ├── assets
    ├── components
    ├── contents
      ├── en
        ├── blog
        ├── blogsEn.js
      ├── es
        ├── blog
        ├── blogsEs.js
    ├── layouts
    ├── locales
    ├── pages
    ├── plugins
    ├── static
    ├── .gitignore
    ├── LICENSE
    ├── nuxt.config.js
    ├── package-lock.json
    ├── package.json
    └── README.md

1.  **`/node_modules`**: This directory contains all of the modules of code that your project depends on (npm packages) are automatically installed.

2.  **`/assets`**: You will find the images and assets for the project. You can find more information at [Nuxt's assets directory documentation](https://nuxtjs.org/guide/assets/)

3.  **`/components`**: Vue components for the project. You can find more information at [Nuxt's components directory documentation](https://nuxtjs.org/guide/directory-structure#the-components-directory)

4.  **`/contents`**: You will save your MD files here. They are divided by language and you will have to write the URL name of each of them at `blogsEn.js` and `blogsEs.js`.

5.  **`/layouts`**: You can find information at [Nuxt's layout directory documentation](https://nuxtjs.org/guide/directory-structure#the-layouts-directory)

6.  **`/locales`**: You will save your translations here.

7.  **`/pages`**: You can find information at [Nuxt's pages directory documentation](https://nuxtjs.org/guide/directory-structure#the-pages-directory)

8.  **`/plugins`**: You can find information at [Nuxt's plugins directory documentation](https://nuxtjs.org/guide/directory-structure#the-plugins-directory)

9.  **`/statics`**: You can find information at [Nuxt's statics directory documentation](https://nuxtjs.org/guide/directory-structure#the-static-directory)

10. **`.gitignore`**: This file tells git which files it should not track / not maintain a version history for.

11. **`LICENSE`**: This is licensed under the MIT license.

12. **`nuxt-config.js`**: This is the main configuration file for a Nuxt site. This is where you can specify information about your site (metadata) like the site title and description, which Nuxt plugins you’d like to include, etc. (Check out the [config docs](https://nuxtjs.org/guide/configuration) for more detail).

13. **`package-lock.json`** (See `package.json` below, first). This is an automatically generated file based on the exact versions of your npm dependencies that were installed for your project. **(You won’t change this file directly).**

14. **`package.json`**: A manifest file for Node.js projects, which includes things like metadata (the project’s name, author, etc). This manifest is how npm knows which packages to install for your project.

15. **`README.md`**: A text file containing useful reference information about your project.