# Guides
Use the following guides to get started using Cosmic with select development libraries.

## Initial Setup
Before doing any coding, let's set up a Bucket with content using the following steps:
1. Create or log in to your [Cosmic account](https://app.cosmicjs.com)
2. Install the [Simple Blog](https://www.cosmicjs.com/apps/simple-blog) by clicking "Select App" then following the steps to create a new Bucket with the demo content. Alternatively, you could start by creating a new Bucket from scratch and add an Object Type titled `Posts` that has the slug `posts` and add a File Metafield titled `Hero` with key `hero`. Then add a few Objects with your own demo content.

Now that we have some demo content, we can integrate the content using the following development tools.

## React
[React](https://reactjs.org/) is a component-based JavaScript library for building user interfaces.

Cosmic makes a great [React CMS](https://www.cosmicjs.com/knowledge-base/react-cms) for your React websites and apps.

Before adding any code, make sure to follow the [Initial Setup](#initial-setup) at the top of this page to set up your content in Cosmic. Then take the following steps to add Cosmic-powered content to your React apps:

### 1. Install a new React app
You can use [Create React App](https://github.com/facebook/create-react-app) to install a new React app that includes tooling and configurations.
```bash
npm i -g create-react-app
create-react-app cosmic-react-app
```
### 2. Install the Cosmic NPM module
```bash
cd cosmic-react-app
npm i cosmicjs
```
### 3. Add the following code into your `src/App.js` file
Find your Bucket slug and API read key in <i>Your Bucket > Basic Settings > API Access</i> after [logging in](https://app.cosmicjs.com).
```javascript
// src/App.js
import React, { useState, useEffect } from 'react'

const Cosmic = require('cosmicjs')
const api = Cosmic()
// Set these values, found in Bucket > Settings after logging in at https://app.cosmicjs.com/login
const bucket = api.bucket({
  slug: 'YOUR_BUCKET_SLUG',
  read_key: 'YOUR_BUCKET_READ_KEY'
})
function App() {
  // Use Hooks to get page data from Cosmic!
  const [data, setData] = useState(null);
  useEffect(() => {
    const fetchBlog = async () => {
      const data = await bucket.getObjects({
        type: 'posts',
        props: 'slug,title,content,metadata' // Limit the API response data by props
      })
      setData(data)
    };
    fetchBlog()
  }, []);
  if (!data)
    return <div>Loading...</div>
  const posts = data.objects
  return <div>
    { posts.map(post => 
      <div key={post.slug} style={{marginBottom: 20}}>
        {
          post.metadata.hero &&
          <div><img alt="" src={`${post.metadata.hero.imgix_url}?w=400`}/></div>
        }
        <div>{post.title}</div>
      </div>)
    }
  </div>
}
export default App;
```

### 4. Start your app
Start your app, and go to http://localhost:3000. Dance 🎉
```
npm start
```

## Node.js
[Node.js](https://nodejs.org/en/) is a JavaScript runtime built on Chrome's V8 engine that allows you to run JavaScript server-side.

Cosmic makes a great [Node.js CMS](https://www.cosmicjs.com/knowledge-base/nodejs-cms) for your Node.js websites and apps.

Before adding any code, make sure to follow the [Initial Setup](#initial-setup) at the top of this page to set up your content in Cosmic. Then take the following steps to add Cosmic-powered content to your Node.js apps:

### 1. Install Express
You can use the popular [Express](https://expressjs.com) website framework to get a Node.js Cosmic website up and running quickly. Start by creating a project folder and installing Express and Cosmic.
```bash
mkdir cosmic-node-app
cd cosmic-node-app
npm i express cosmicjs
```
### 4. Create an `index.js` file
Find your Bucket slug and API read key in <i>Your Bucket > Basic Settings > API Access</i> after [logging in](https://app.cosmicjs.com).
```javascript
// index.js
const express = require('express')
const app = express()
const Cosmic = require('cosmicjs')
const api = Cosmic()
const PORT = process.env.PORT || 3000
// Set these values, found in Bucket > Settings after logging in at https://app.cosmicjs.com/login
const bucket = api.bucket({
  slug: 'YOUR_BUCKET_SLUG',
  read_key: 'YOUR_BUCKET_READ_KEY'
})
app.get('*', async (req, res) => {
  const data = await bucket.getObjects({
    type: 'posts',
    props: 'slug,title,content,metadata' // Limit the API response data by props
  })
  const posts = data.objects
  let markup = ``
  posts.map(post => {
    markup += `<div style="margin-bottom: 20px">
    <div><img alt="" src="${post.metadata.hero.imgix_url}?w=400"/></div>
    <div>${post.title}</div>
    </div>`
  })
  res.set('Content-Type', 'text/html')
  res.send(markup)
})
app.listen(PORT, () => { 
  console.log('Your Cosmic app is running at http://localhost:' + PORT)
})
```

### 4. Start your app
Start your app, and go to http://localhost:3000. Dance 🎉
```
node index.js
```

## Vue.js
[Vue.js](https://vuejs.org/) is a progressive JavaScript framework for building user interfaces.

Cosmic makes a great [Vue CMS](https://www.cosmicjs.com/knowledge-base/vuejs-cms) for your Vue websites and apps.

Before adding any code, make sure to follow the [Initial Setup](#initial-setup) at the top of this page to set up your content in Cosmic. Then take the following steps to add Cosmic-powered content to your Vue.js apps:

### 1. Install a new Vue app
You can use the [Vue CLI](https://cli.vuejs.org/) to install a new Vue app that includes tooling and configurations.
```bash
npm install -g @vue/cli
vue create cosmic-vue-app
```
### 2. Install the Cosmic NPM module
```bash
cd cosmic-vue-app
npm i cosmicjs
```
### 3. Add the following code into your `src/App.vue` file
Find your Bucket slug and API read key in <i>Your Bucket > Basic Settings > API Access</i> after [logging in](https://app.cosmicjs.com).
```javascript
// src/App.vue
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>Cosmic Vue App</h1>
    <div v-if="loading">Loading...</div>
    <ul>
      <li v-for="post in posts" :key="post.slug">
        <div>{{ post.title }}</div>
        <img alt="" :src="post.metadata.hero.imgix_url + '?w=400'"/>
      </li>
    </ul>
  </div>
</template>
<script>
const Cosmic = require('cosmicjs')
const api = Cosmic()
// Set these values, found in Bucket > Settings after logging in at https://app.cosmicjs.com/login
const bucket = api.bucket({
  slug: 'YOUR_BUCKET_SLUG',
  read_key: 'YOUR_BUCKET_READ_KEY'
})
export default {
  name: 'App',
  data () {
    return {
      loading: false,
      posts: null
    }
  },
  created () {
    this.fetchData()
  },
  methods: {
    fetchData () {
      this.error = this.post = null
      this.loading = true
      bucket.getObjects({
        type: 'posts',
        props: 'slug,title,content,metadata' // Limit the API response data by props
      }).then(data => {
        const posts = data.objects
        this.loading = false
        this.posts = posts
      })
    }
  }
}
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
}
</style>
```

### 4. Start your app
Start your app, and go to http://localhost:3000. Dance 🎉
```
npm run serve
```

## Next.js
[Next.js](https://nextjs.org/) is a framework for building React websites and apps.

Cosmic makes a great [Next.js CMS](https://www.cosmicjs.com/knowledge-base/nextjs-cms) for your Next.js websites and apps.

Before adding any code, make sure to follow the [Initial Setup](#initial-setup) at the top of this page to set up your content in Cosmic. Then take the following steps to add Cosmic-powered content to your Next.js apps:

### 1. Install a new Next.js app
You can use [Create Next App](https://nextjs.org/docs#setup) to install a new Next.js app that includes tooling and configurations. When prompted, select default starter app.
```bash
npm i -g create-next-app
create-next-app cosmic-next-app
```
### 2. Install the Cosmic NPM module
```bash
cd cosmic-next-app
npm i cosmicjs
```
### 3. Add the following code into your `pages/index.js` file
Find your Bucket slug and API read key in <i>Your Bucket > Basic Settings > API Access</i> after [logging in](https://app.cosmicjs.com).
```javascript
// pages/index.js
import Head from 'next/head'
const Cosmic = require('cosmicjs')
const api = Cosmic()
// Set these values, found in Bucket > Settings after logging in at https://app.cosmicjs.com/login
const bucket = api.bucket({
  slug: 'YOUR_BUCKET_SLUG',
  read_key: 'YOUR_BUCKET_READ_KEY'
})
function Blog({ posts }) {
  return (
    <div className="container">
      <Head>
        <title>Cosmic App</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>
      {posts.map((post) => (
        <div key={post.slug}>
          <h3>{post.title}</h3>
          <img alt="" src={`${post.metadata.hero.imgix_url}?w=400`}/>
        </div>
      ))}
    </div>
  )
}
export async function getStaticProps() {
  const data = await bucket.getObjects({
    type: 'posts',
    props: 'slug,title,content,metadata' // Limit the API response data by props
  })
  const posts = await data.objects
  return {
    props: {
      posts,
    }
  }
}
export default Blog
```

### 4. Start your app
Start your app, and go to http://localhost:3000. Dance 🎉
```
npm run dev
```

## Nuxt.js
[Nuxt.js](https://nuxtjs.org/) is a framework for building Vue websites and apps.

Cosmic makes a great [Nuxt.js CMS](https://www.cosmicjs.com/knowledge-base/nuxtjs-cms) for your Nuxt.js websites and apps.

Before adding any code, make sure to follow the [Initial Setup](#initial-setup) at the top of this page to set up your content in Cosmic. Then take the following steps to add Cosmic-powered content to your Nuxt.js apps:

### 1. Install a new Nuxt.js app
You can use [Create Nuxt App](https://github.com/nuxt/create-nuxt-app) to install a new Nuxt.js app that includes tooling and configurations.
```bash
npm i -g create-nuxt-app
create-nuxt-app cosmic-nuxt-app
```
### 2. Install the Cosmic NPM module
```bash
cd cosmic-nuxt-app
npm i cosmicjs
```
### 3. Add the following code into your `pages/index.vue` file
Find your Bucket slug and API read key in <i>Your Bucket > Basic Settings > API Access</i> after [logging in](https://app.cosmicjs.com).
```javascript
// pages/index.vue
<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">
        cosmic-nuxt-app
      </h1>
      <div v-if="loading">Loading...</div>
      <div v-for="post in posts" :key="post.slug">
        <h3>{{ post.title }}</h3>
        <img alt="" :src="post.metadata.hero.imgix_url + '?w=400'"/>
      </div>
    </div>
  </div>
</template>
<script>
const Cosmic = require('cosmicjs')
const api = Cosmic()
// Set these values, found in Bucket > Settings after logging in at https://app.cosmicjs.com/login
const bucket = api.bucket({
  slug: 'YOUR_BUCKET_SLUG',
  read_key: 'YOUR_BUCKET_READ_KEY'
})
export default {
  data() {
    return {
      loading: true
    }
  },
  async asyncData () {
    const data = await bucket.getObjects({
      type: 'posts',
      props: 'slug,title,content,metadata' // Limit the API response data by props
    })
    const posts = await data.objects
    return { 
      posts,
      loading: false
    }
  }
}
</script>
<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}
</style>
```

### 4. Start your app
Start your app, and go to http://localhost:3000. Dance 🎉
```
npm run dev
```

## Gatsby
[Gatsby](https://gatsbyjs.org/) is a framework for building React websites and apps.

Cosmic makes a great [Gatsby CMS](https://www.cosmicjs.com/knowledge-base/gatsby-cms) for your Gatsby websites and apps.

Before adding any code, make sure to follow the [Initial Setup](#initial-setup) at the top of this page to set up your content in Cosmic. Then take the following steps to add Cosmic-powered content to your Gatsby apps:

### 1. Install a new Gatsby app
You can use the Gatsby CLI to install a new Gatsby app that includes tooling and configurations.
```bash
npm install -g gatsby-cli
gatsby new cosmic-gatsby-app
```
### 2. Install the Cosmic source plugin for Gatsby
Install the [Cosmic source plugin for Gatsby](https://www.npmjs.com/package/gatsby-source-cosmicjs).
```bash
cd cosmic-gatsby-app
yarn add gatsby-source-cosmicjs
```
### 3. Add the following code into your `gatsby-config.js` file in the plugins section.
Find your Bucket slug and API read key in <i>Your Bucket > Basic Settings > API Access</i> after [logging in](https://app.cosmicjs.com).
```javascript
// In your gatsby-config.js
plugins: [
  {
    resolve: `gatsby-source-cosmicjs`,
    options: {
      bucketSlug: `YOUR_BUCKET_SLUG`, // Get this value in Bucket > Settings
      objectTypes: [`posts`], // Note it will result in GraphQL queries (allCosmicjsPosts, cosmicjsPosts)
      // If you have enabled read_key to fetch data (optional).
      apiAccess: {
        read_key: `YOUR_BUCKET_READ_KEY`, // Get this value in Bucket > Settings
      },
      localMedia: true // Download media locally for gatsby image (optional)
    }
  },
]
```

### 4. Add the following code into your `gatsby-node.js` file

```javascript
const path = require(`path`)

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  // Get the single post layout file
  const blogPost = path.resolve(`./src/templates/blog-post.js`)
  // Query the GraphQL to get our posts
  const result = await graphql(
    `
      {
        allCosmicjsPosts(sort: { fields: [created], order: DESC }, limit: 1000) {
          edges {
            node {
              slug,
              title
            }
          }
        }
      }
    `
  )

  if (result.errors) {
    throw result.errors
  }

  // Create blog posts pages.
  const posts = result.data.allCosmicjsPosts.edges

  // For each post in posts create a separate page
  posts.forEach((post, index) => {
    createPage({
      path: post.node.slug,
      component: blogPost,
      context: {
        slug: post.node.slug
      },
    })
  })
}
```

### 5. Create `blog-post.js` in `src/templates/` directory and add following code

This is the layout for a single blog post page which we used in `gatsby-node.js` 

```javascript
import React from "react"
import { graphql } from "gatsby"

const BlogPostTemplate = ({ data }) => {
  const post = data.cosmicjsPosts // get the post data from query

  // Render the post data  
  return (
    <article>
      <h1>{post.title}</h1>
      <small>{post.created}</small>
      <div><img alt="" src={`${post.metadata.hero.imgix_url}?w=400`}/></div>
      <section dangerouslySetInnerHTML={{ __html: post.content }} />
    </article>
  )
}

export default BlogPostTemplate

// Query to get single Post where slug is equal
export const pageQuery = graphql`
  query BlogPostBySlug($slug: String!) {
    cosmicjsPosts(slug: { eq: $slug }) {
      id
      content
      title
      metadata {
        hero {
          imgix_url
        }
      }
      created(formatString: "MMMM DD, YYYY")
    }
  }
`
```

### 6. Edit `index.js` file in `src/pages/` directory and add following code

```javascript
import React from "react"
import { Link, graphql } from "gatsby"

const BlogIndex = ({ data }) => {
  const posts = data.allCosmicjsPosts.edges // getting all posts from query

  // Rendering list of posts with link to their url
  return (
    <div>
      {posts.map(({ node }) => {
        return (
          <div key={node.slug}>
            <Link to={node.slug}>
              <h3>{node.title}</h3>
              <img alt="" src={`${node.metadata.hero.imgix_url}?w=400`}/>
            </Link>
          </div>
        )
      })}
    </div>
  )
}

export default BlogIndex

// Query all posts from GraphQL
export const pageQuery = graphql`
  query {
    allCosmicjsPosts(sort: { fields: [created], order: DESC }, limit: 1000) {
      edges {
        node {
          slug
          title
          metadata {
            hero {
              imgix_url
            }
          }
          created(formatString: "DD MMMM, YYYY")
        }
      }
    }
  }
`
```



### 7. Start your app

Start your app, and go to http://localhost:8000. Dance 🎉
```
 yarn develop
```

## Angular

[Angular](https://angular.io/) is a JavaScript library for building user interfaces.

Cosmic makes a great [Angular CMS](https://www.cosmicjs.com/knowledge-base/angularjs-cms) for your Angular websites and apps.

Before adding any code, make sure to follow the [Initial Setup](#initial-setup) at the top of this page to set up your content in Cosmic. Then take the following steps to add Cosmic-powered content to your Angular apps:

### 1. Install a new Angular app
Install the Angular CLI to create a project that includes tooling and configurations.
```bash
npm install -g @angular/cli
ng new cosmic-angular-app
```
### 2. Install the Cosmic NPM module
```bash
cd cosmic-angular-app
npm i cosmicjs
```
### 3. Add the following code into your `src/app/app.component.ts` file
Find your Bucket slug and API read key in <i>Your Bucket > Basic Settings > API Access</i> after [logging in](https://app.cosmicjs.com).
```javascript
// src/app/app.component.ts
import {Component, OnInit} from '@angular/core';
import Cosmic from 'cosmicjs';

@Component({
  selector: 'app-root',
  template: `
    <div id="app">
      <h1>Cosmic Angular App</h1>
      <div *ngIf="loading">Loading...</div>
      <ul>
        <li *ngFor="let post of posts">
          <div>{{ post.title }}</div>
          <img alt="" [src]="post.metadata.hero.imgix_url + '?w=400'"/>
        </li>
      </ul>
    </div>
  `
})
export class AppComponent implements OnInit{
  title = 'cosmic-angular-app';
  bucket = null;
  loading = false;
  posts = [];

  constructor(
  ) {
    // Set these values, found in Bucket > Settings after logging in at https://app.cosmicjs.com/login
    this.bucket = Cosmic().bucket({
      slug: 'YOUR_BUCKET_SLUG',
      read_key: 'YOUR_BUCKET_READ_KEY'
    });
  }

  async ngOnInit() {
    this.loading = true;

    await this.bucket.getObjects({
      type: 'posts',
      props: 'slug,title,content,metadata' // Limit the API response data by props
    }).then((response) => {
      this.posts = response.objects;
    });

    this.loading = false;
  }
}
```
### 4. Edit `src/polyfills.ts` and add the following code
```javascript
// src/polyfills.ts
(window as any).process = {
    env: { DEBUG: undefined },
};
```
### 5. Start your app
Start your app, and go to http://localhost:4200. Dance 🎉
```
ng serve --open
```

## Ruby on Rails

[Ruby on Rails](https://rubyonrails.org/) is a server-sided framework written in Ruby, widely used by startups to iterate quickly.

Cosmic makes a great [Ruby on Rails CMS](https://www.cosmicjs.com/knowledge-base/ruby-on-rails-cms) for your Ruby on Rails websites and apps.

Before adding any code, make sure to follow the [Initial Setup](#initial-setup) at the top of this page to set up your content in Cosmic. Then take the following steps to add Cosmic-powered content to your Ruby on Rails apps:

### 1. Create a new Ruby on Rails app
If you don't have Ruby on Rails installed on your machine, you may need to start with:
```
gem install rails
```
and
```
gem install bundler
```
Create a new Ruby on Rails application by using the following commands in the terminal:
```bash
rails new cosmic-app --skip-active-record
cd cosmic-app
```

### 2. Install HTTParty Gem to make HTTP Requests
In the `Gemfile`, add the following line of code to the bottom of the file:
```ruby
# Gemfile
gem 'httparty'
```
and to install the `HTTParty` gem run:
```
bundle install
```

### 3. Adding Cosmic Credentials to Rails app
To use your Read/Write Key and Slug of your Cosmic Bucket in a secure way in Rails, please run the following command on terminal:
```bash
EDITOR="vim" rails credentials:edit
````

Paste the following yml configuration in the text editor and save the file:
```yml
cosmic:
  slug: <add cosmic bucket slug here>
  read_key: <add cosmic bucket read key here>
```

### 4. Configure Autoloading for API Library
In `config/application.rb`, please add the following line in the class `Application < Rails::Application` to autoload API Wrapper when server starts. Add this line of code under `config.load_defaults`.
```ruby
# config/application.rb
config.autoload_paths << Rails.root.join('lib')
```

### 5. Add the code for the API Wrapper
Add a new file structure in the `lib` folder.
```terminal
cd lib && mkdir api_wrappers && cd api_wrappers && mkdir cosmic && cd cosmic && touch objects_wrapper.rb

cd ../../..
```

In the newly created `lib/api_wrappers/cosmic/objects_wrapper.rb` file, paste the following code:
```ruby
# lib/api_wrappers/cosmic/objects_wrapper.rb
module ApiWrappers
  module Cosmic
    class ObjectsWrapper
      BASE_URI = 'https://api.cosmicjs.com/v1/'
      COSMIC_CREDENTIALS = Rails.application.credentials.cosmic

      def initialize
        @slug = COSMIC_CREDENTIALS[:slug]
        @read_key = COSMIC_CREDENTIALS[:read_key]
      end

      def fetch_posts
        fetch_objects('posts')
      end

      private

      def fetch_objects(type)
        resource = "#{@slug}/objects"
        params = {
          props: 'slug,title,type_slug,metadata',
          read_key: @read_key,
          type: type
        }

        response = get(resource, params)
        return [] unless response.success?

        response.parsed_response['objects']
      end

      def get(resource, params)
        make_request('get', resource, query: params)
      end

      def make_request(method, resource, params)
        uri = "#{BASE_URI}#{resource}"
        HTTParty.send(method, uri, params)
      end
    end
  end
end
```

### 6. Add Controller
Run the following command in the terminal to generate a controller with an index method and view for index.
```bash
rails g controller posts index
```

Go to `config/routes.rb` and set the newly created index method of `PostsController` as the root path. The code in the `routes.rb` would look like the one given below.
```ruby
Rails.application.routes.draw do
  get 'posts/index'
  root to: 'posts#index'
end
```

Update the `app/controllers/posts_controller.rb` with the following code:
```ruby
class PostsController < ApplicationController
  before_action :set_cosmic_objects_wrapper

  def index
    @posts = @cosmic_objects_wrapper.fetch_posts
    puts @posts
  end

  private

  def set_cosmic_objects_wrapper
    @cosmic_objects_wrapper = ApiWrappers::Cosmic::ObjectsWrapper.new
  end
end
```

### 6. Update Views
Render your posts in the `app/views/posts/index.html.erb` with the following code:
```erb
<% @posts.each do |post| %>
  <div key=<%= post['slug']%> style='margin-bottom:20px;'>
    <% if post['metadata'] && post['metadata']['hero'] %>
      <div>
        <img alt='' src='<%= post['metadata']['hero']['imgix_url'] %>?w=800&auto=format' width='400px' />
      </div>
    <% end %>

    <div><%= post['title'] %></div>
  </div>
<% end %>
```

### 7. Start your app
Start your app, and go to http://localhost:3000. Dance 🎉
```
rails s
```

## Go

[Go](https://golang.org/) is an open source programming language that makes it easy to build **simple**, **reliable**, and **efficient** software.

Cosmic makes a great [Go CMS](https://www.cosmicjs.com/knowledge-base/go-cms) for your websites and apps.

Before adding any code, make sure to follow the [Initial Setup](#initial-setup) at the top of this page to set up your content in Cosmic. Then take the following steps to add Cosmic-powered content to your Go apps:

### 1. Go app setup
If you don't have Go installed on your machine, you may need to start by installing Go by following the [documentation](https://golang.org/doc/install).

After everything is ready, create a folder which will contain all of our code:
```bash
mkdir go-cosmic-app
cd go-cosmic-app
```

### 2. Install `godotenv` package
```bash
go mod init go-cosmic-app
go get github.com/joho/godotenv
```

### 3. Create `.env`
Create a `.env` file and add cosmic bucket configuration. You can find your Bucket slug and API read key in <i>Your Bucket > Basic Settings > API Access</i> after [logging in](https://app.cosmicjs.com).
```bash
# Set these values, found in Bucket > Settings after logging in at https://app.cosmicjs.com/
BUCKET_SLUG= # Required
READ_KEY= # Required if activated in the bucket
```
### 4. Create HTTP Server and Route
Create a `app.go` file and paste the following code:
```go
// app.go
package main

import (
	"encoding/json"
	"fmt"
	"html/template"
	"io/ioutil"
	"log"
	"net/http"
	"github.com/joho/godotenv"
	"os"
)

// Data is a array of objects from Cosmic API
type Data struct {
	Objects []Post
}

// Post is a representation of post object
type Post struct {
	Title    string
	Slug     string
	Content  template.HTML
	Metadata Metadata
}

// Metadata is a representation of metadata object
type Metadata struct {
	Hero Image
}

// Image is a object of URL & ImgixURL
type Image struct {
	URL      string
	ImgixURL string `json:"imgix_url"`
}

func indexHandler(w http.ResponseWriter, r *http.Request) {
  if r.URL.Path != "/" {
    http.Error(w, "404 not found.", http.StatusNotFound)
    return
  }

  if r.Method != "GET" {
    http.Error(w, "Method is not supported.", http.StatusNotFound)
    return
  }

  if ok := checkIfEnvExists("BUCKET_SLUG"); !ok {
    http.Error(w, "BUCKET_SLUG is not present in the .env", http.StatusInternalServerError)
    return
  }

  var readKey string
  if ok := checkIfEnvExists("READ_KEY"); ok {
    readKey = "&read_key=" + os.Getenv("READ_KEY")
  }

  bucketSlug := os.Getenv("BUCKET_SLUG")

  apiURL := "https://api.cosmicjs.com/v1/"
  url := apiURL + bucketSlug + "/objects?&hide_metafields=true&type=posts&props=slug,title,content,metadata" + readKey

  res, err := http.Get(url)
  var data Data

  if err != nil {
    log.Println(err)
  } else {
    body, err := ioutil.ReadAll(res.Body)
    if err != nil {
      log.Println(err)
    } else {
      json.Unmarshal(body, &data)
    }
  }

  t, _ := template.ParseFiles("index.html")

  t.Execute(w, data)
}

func getPortEnv() string {
  var port string
  var ok bool

  if port, ok = os.LookupEnv("PORT"); !ok {
    port = "8080"
  }

  return ":" + port
}

func checkIfEnvExists(key string) bool {
  var ok bool
  if _, ok = os.LookupEnv(key); !ok {
    return false
  }
  return true
}

func main() {
  http.HandleFunc("/", indexHandler)

  if err := godotenv.Load(); err != nil {
    log.Fatal("Error loading .env file")
  }

  port := getPortEnv()

  fmt.Println("Starting server at port", port)
  if err := http.ListenAndServe(port, nil); err != nil {
    log.Fatal(err)
  }
}
```

### 5. Add template
Create `index.html` file and paste the following code:
```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Go Cosmic Blog</title>
  <meta name="description" content="Go Cosmic Blog">
  <meta name="author" content="Cosmic">
  <style>
    body {
      font-family: Avenir, Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: left;
      color: #2c3e50;
    }
  </style>
</head>
<body>
  <div>
    <h1>
      Go Cosmic Blog
    </h1>
    {{if not .Objects}}
      <h2>No post found</h2>
    {{end}}
    {{range .Objects}}
    <article>
      <h3>{{ .Title }}</h3>
      <img alt="{{.Title}}" src="{{.Metadata.Hero.ImgixURL}}?w=400"/>
      {{.Content}}
    </article>
    {{end}}
  </div>
</body>
</html>
```

### 6. Start your app
Start your app, and go to http://localhost:8080. Dance 🎉
```bash
go run app.go
```

## Java

[Java](https://www.java.com/en/) is a class-based, object-oriented programming language that is designed to have as few implementation dependencies as possible.

Before adding any code, make sure to follow the [Initial Setup](#initial-setup) at the top of this page to set up your content in Cosmic. Then take the following steps to add Cosmic-powered content to your Java apps:

### 1. Downloading the essentials
1. Download and Install Java 1.8 JDK on your computer. You can follow this [guide](https://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html) or any other material on the web for the installation.
2. Download and install the [IntelliJ Community edition](https://www.jetbrains.com/idea/download/). We will be using IntelliJ IDE for this guide.

### 2. Create a new Java Spring Boot Project
1. Go to [https://start.spring.io](https://start.spring.io) where you can see the spring Initializer page to create spring Boot application
2. The only things you want to change in this form is the artifact name, you can call this “cosmicapp” in Artifact. Also, select Java 8 at the bottom of the page.
3. Click Generate the project and you will get a zip file named `cosmicapp.zip`.
4. Unzip anywhere on your computer, and you can find a folder named `cosmicapp`.
5. Open your IntelliJ IDE and select “Import Project” option in the welcome screen.
6. Change folder path to your cosmicapp folder which you just extracted, and click OK.
7. Select “Import Project from external model”, and select Maven. (if you don’t see Maven, make sure you install IntelliJ Maven plugin, link: https://www.jetbrains.com/help/idea/maven-support.html).

### 3. Configure the `pom.xml` file to manage the dependencies
We now need to add the following dependencies into maven’s configuration file. Copy the below content into your `pom.xml` file.
```xml
#pom.xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.3.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>demo1</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo1</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
````
Right-Click on `pom.xml` file and Select (Maven -> Reimport) to import all the dependencies in your project.

### 4. Adding Cosmic Credentials to Java Spring Boot Project
Inside the folder `src/main/resources`, add the Cosmic credentials to the `application.properties` file. You can find your Bucket slug and API read key in <i>Your Bucket > Basic Settings > API Access</i> after [logging in](https://app.cosmicjs.com).
```properties
#src/main/resources/application.properties
slug = <add cosmic bucket slug here>
read_key = <add cosmic bucket read key here>
```

### 5. Create the service component of Java Spring Boot App
Inside the folder `src/main/java`, right-click on `com.example.cosmicapp` package and add a new package named `service` by selecting (New -> Package).

Now right-click on `com.example.cosmicapp.service` package, select (New -> Class) and add a Java class named `JsonParsingService.java`.
```java
# src/main/java/com/example/cosmicapp/service/JsonParsingService.java
package com.example.cosmicapp.service;
import org.springframework.stereotype.Service;
import org.springframework.web.client.RestTemplate;

@Service
public class JsonParsingService {
    private RestTemplate restTemplate = new RestTemplate();
    public Object parse(String url) {
        return restTemplate.getForObject(url, Object.class);
    }
}
```

### 6. Create the domain component of Java Spring Boot App 
Inside the folder `src/main/java`, right-click on `com.example.cosmicapp` package and add a new package named `domain` by selecting (New -> Package).

1. Right click on `com.example.cosmicapp.domain` package, select (New -> Class), and add Java Class named `Bucket.java`.
```java
#src/main/java/com/example/cosmicapp/domain/Bucket.java
package com.example.cosmicapp.domain;
import com.fasterxml.jackson.annotation.JsonIgnoreProperties;
import java.util.List;

@JsonIgnoreProperties(ignoreUnknown = true)
public class Bucket {
    public List<Object> objects;
}
```
2. Create another Java class `Object.java` inside the package `com.example.cosmicapp.domain` in a similar way as above.
```java
#src/main/java/com/example/cosmicapp/domain/Object.java
package com.example.cosmicapp.domain;
import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public class Object {
    public String slug;
    public String title;
    public String content;
    public Metadata metadata;

    public Metadata getMetadata() {
        return metadata;
    }
    public void setMetadata(Metadata metadata) {
        this.metadata = metadata;
    }
    public String getTitle() {
        return title;
    }
    public void setTitle(String title) {
        this.title = title;
    }
    public String getContent() {
        return content;
    }
    public void setContent(String content) {
        this.content = content;
    }
    public String getSlug() {
        return slug;
    }
    public void setSlug(String slug) {
        this.slug = slug;
    }
}
```
3. Add another Java class `Metadata.java` inside the package `com.example.cosmicapp.domain` in a similar way as above.
```java
#src/main/java/com/example/cosmicapp/domain/Metadata.java
package com.example.cosmicapp.domain;
import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public class Metadata {
    public Hero hero;
    
    public Hero getHero() {
        return hero;
    }
    public void setHero(Hero hero) {
        this.hero = hero;
    }
}
```
4. Add the last Java class `Hero.java` inside the package `com.example.cosmicapp.domain` in a similar way as above.
```java
#src/main/java/com/example/cosmicapp/domain/Hero.java
package com.example.cosmicapp.domain;
import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public class Hero {
    public String url;
    public String imgix_url;

    public String getUrl() {
        return url;
    }
    public void setUrl(String url) {
        this.url = url;
    }
    public String getImgix_url() {
        return imgix_url;
    }
    public void setImgix_url(String imgix_url) {
        this.imgix_url = imgix_url;
    }
}
```


### 7. Add the Controller component to the Java Spring Boot App
Inside the folder `src/main/java`, right-click on `com.example.cosmicapp` package and add a new package named `controller` by selecting (New -> Package).

Now right-click on `com.example.cosmicapp.controller` package, select (New -> Class), and add Java class named `Bucket.java`.
```java
#src/main/java/com/example/cosmicapp/controller/MainController.java
package com.example.cosmicapp.controller;
import com.example.cosmicapp.domain.Bucket;
import com.example.cosmicapp.domain.Object;
import com.example.cosmicapp.service.JsonParsingService;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.core.env.Environment;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.util.UriComponentsBuilder;
import java.util.LinkedHashMap;

@Controller
public class MainController {
    @Autowired
    private Environment env;
    private static final String MAIN_PAGE = "main";
    private static final String baseURL = "https://api.cosmicjs.com/v1/";
    private static final String type = "posts";
    private static final String props = "slug,title,content,metadata,";
    @Autowired
    JsonParsingService jsonParsingService;

    @GetMapping
    public String main(final Model model) {
        String url = baseURL + env.getProperty("slug") + "/objects";
        UriComponentsBuilder builder = UriComponentsBuilder.fromHttpUrl(url)
                .queryParam("type", type)
                .queryParam("read_key", env.getProperty("read_key"))
                .queryParam("props", props);
        LinkedHashMap<String, Object> objectsLinkedHashMap = (LinkedHashMap<String, Object>) jsonParsingService.parse(builder.toUriString());
        ObjectMapper mapper = new ObjectMapper();
        Bucket bucket = mapper.convertValue(objectsLinkedHashMap, Bucket.class);
        model.addAttribute("objects", bucket.objects);
        return MAIN_PAGE;
    }
}
```

### 8. Update Views
Render your posts in the `src/main/resources/templates/main.html` with the following code:

```html
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org">
<head>
    <title>List of Posts</title>

    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"></link>
</head>
<body>
<h1> List of Posts</h1>
<div class="container">
    <div th:each="post : ${objects}">
        <div th:if="${post.metadata != null && post.metadata.hero != null} ">
            <img th:src="${post.metadata.hero.url}" width='500px' />
        </div>
        <div th:text="${post.title}"></div>
        <br><br>
    </div>
</div>

<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</body>
</html>
```

### 9. Start your app
Start your Spring Boot application by running the command below, and go to http://localhost:8080. Dance 🎉
```terminal
mvn spring-boot:run 
```

## .Net

::: tip COMING SOON!
If you would like to contribute a demo using this development tool [fork the repo and issue a pull request](https://github.com/cosmicjs/docs).
:::

## Python

::: tip COMING SOON!
If you would like to contribute a demo using this development tool [fork the repo and issue a pull request](https://github.com/cosmicjs/docs).
:::
