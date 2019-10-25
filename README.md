<p align="center">
  <a href="https://nuxtjs.org/">
  <img src="https://avatars2.githubusercontent.com/u/23360933?s=200&v=4" height="60">
  </a>
  +
  <img src="https://geekytheory.com/wp-content/uploads/2014/03/markdown_inte-1024x630.png" height="60">
</p>
<h1 align="center">
  Workshop: Building a multilingual blog using Vue.js, Nuxt and Markdown
</h1>
<p align="center">
  Just follow the steps and ask <a href="https://twitter.com/MarinaAisa">Marina Aísa</a> anything you want during the workshop or afterwards.
</p>

## ⚡️ Live
[Check how the final result should look like](https://nuxt-markdown-blog-starter.netlify.com/)

## 👉 Step 0: Preferably, do it before the workshop starts

1.  You need to install Node first in your computer

    **For Mac:**

    Install Homebrew (if you don't have it yet)

    ```sh
      /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```
    and then install Node through Homebrew:

    ```sh
    brew install node
    ```

    **For Windows** you can download it [here](https://nodejs.org/es/download/).

## 👉 Step 1: Download the project and get started

1.  **Fork** this repository clicking on the `Fork` button on the top right of this page.

2.  Clone your forked repository to download it in your local machine.

    ```sh
    git clone https://github.com/[YOUR-USERNAME]/workshop-nuxt-blog.git
    ```

3.  Go to the project and install npm.

    ```sh
    cd workshop-nuxt-blog/
    npm install
    git fetch
    ```

4.  Run a node server working in your local machine to see your following changes

    ```sh
    npm run dev
    ```

    Your site is now running at `http://localhost:3000`!

## 👉 Step 2: Create your first Markdown file in English

1.  Go to `contents/en/blog/` to create your blog post in Markdown (.md) with this syntaxis:

    **Important: the name of this file will be the name of the URL and it has to be the same as the `name` property inside the Markdown's frontmatter**
    
    Example:

    ```
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

2. Go to `contents/en/blogsEn.js` and write the `name` of your blog post inside the exported array. Example:

    ```js
      export default [
        'bacon-ipsum',
      ]
    ```

## 👉 Step 2: Translate it to Spanish

1. Do the same inside `contents/es/blog/` and write a new markdown file with your translated blog post. Remember to translate the name of the file as well. Example:

**Important: `id` has to be the same as the one in English.**

    ```
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

1. Do the same as in English, go to `contents/es/blogsEs.js` and write the `name` of your blog post inside the exported array. Example:

    ```js
      export default [
        'jamon-ipsum',
      ]
    ```

## 👉 Step 3: Add a webpack loader for your Markdown files

1. Install `frontmatter-markdown-loader`:

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

## 👉 Step 4: Import your Markdown files

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

2. Go to `pages/blog/_slug` and inside the `asyncData` function add:

    ```js
    const fileContent = await import(`~/contents/${app.i18n.locale}/blog/${params.slug}.md`)
    const attr = fileContent.attributes
    return {
      name: params.slug,
      title: attr.title,
      trans: attr.trans,
      year: attr.year,
      id: attr.id,
      description: attr.description,
      renderFunc: fileContent.vue.render,
      staticRenderFuncs: fileContent.vue.staticRenderFns,
      image: {
        main: attr.image && attr.image.main,
        og: attr.image && attr.image.og
      }
    }
    ```

## 👉 Step 4: Add transitions

1. Go to `assets/css/base/_general.scss`and add the following code:

    ```css
    .slide-fade-enter-active,
    .slide-fade-leave-active {
      transition: transform .4s, opacity .4s;
    }
    .slide-fade-enter,
    .slide-fade-leave-to {
      transform: translateX(50px);
      opacity: 0;
    }
    ```

2. Go to `pages/blog/_slug` and add inside the `export default` object:

    ```js
    transition: {
      name: 'slide-fade'
    },
    ```

## 👉 Step 5: Deploy on Netlify

1. Commit your local changes to your own repository

    ```sh
    git add .
    git commit -m "Upload new changes"
    git push
    ```

2. Create an account for free on [Netlify](netlify.com)

3. Click the button `New site from Github` in your dashboard page on Netlify, select your repository and follow the instructions.

4. You will get your page online!

## The result

You can find the final code [here](https://github.com/marinaaisa/nuxt-markdown-blog-starter). Psss, it's an **open source code**, so you can use it for any of your projects.