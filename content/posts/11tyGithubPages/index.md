---
title: How to Setup a 11TY Blog on Github Pages
date: 2023-03-03
tags:
  - webdev
cover:
    image: "cover.png"
---

I recently set up this website to write and host resources about my studies, as well as my current gamedev projects. In the past I used overkill solutions like Wordpress to host information about my already abandoned gamedev projects. Later I discovered the static site generator Publii, which I used for quite awhile. And I would still recommend it, especially for non-tech savyy people.

But this time I wanted to go for a more minimalistic approach, so I fired up Google and did some research on 'best static site generator reddit'. The most popular solutions seem to include Jekyll, Hugo and 11ty. Being a JavaScript fanboy I had too choose 11ty.

## Setting up the 11ty page on your local machine
[11ty](https://www.11ty.dev/) has pretty good resources on how to setup a project in general. I decided to base my website on [this](https://github.com/11ty/eleventy-base-blog) example project provided by 11ty.

After setting up the base project make sure that your package.json includes a build script like this:

```
"scripts": {
		"build": "npx @11ty/eleventy"
	}
```

## Deploying to GitHub Pages
- Create a .github/workflows/build.yml file with the following content:

```
name: Build Eleventy

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Setup node
        uses: actions/setup-node@v3
        with:
          node-version: 16

      - name: Install Dependencies
        run: npm install
      
      - name: Build _site
        run: npm run build

      - name: Deploy
        if: success()
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{secrets.GITHUB_TOKEN}}
          publish_dir: ./_site
          # Optional cname if you want to 
          # use a custom domain with Github Pages
          # cname: <subDomain>.<yourDomain>.com 
```

Create your git repository and push your working project to the master branch - if you use main, you have to edit the build.yml file accordingly. Now Github Actions should automatically execute your .yml file.

If you get an error 403 in the output of the GitHub actions go to Settings &rarr; Actions &rarr; General &rarr; Workflow permissions and make sure that 'Read and write permissions' and 'Allow GitHub Actions to create and approve pull requests' are checked.

Now go to Settings &rarr; Pages and under Build and deployment set the branch to gh-pages. Your page should now live on http(s)://\<username>.github.io/\<repository>. I went a step further and [connected the website to my custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site).