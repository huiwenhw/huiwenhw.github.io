---
layout: post
title: Setting up site using Jekyll & Github pages
categories: normal
--- 

I'm assuming that you know Git, Github & Github pages! If you don't, [here's](http://jmcglone.com/guides/github-pages/) a pretty good site to get you started (:

Soo let's start!! There're a few things we need to do:
1. Create a github page 
2. Set up Jekyll
3. Add HTTPS to our site (Won't be touching on it in this post)

P.S. I like to use the terminal, so everything's done in cli. 

### 1. Create a [github page](https://pages.github.com/)
1. Create a github repo, with the repo name as username.github.io (mine's huiwenhw.github.io) 
2. Clone the repository 
```
$ git clone https://github.com/username/username.github.io
```
3. cd into the repo folder & create a index.html file (which is the default page for your site) 
```
$ cd username.github.io
$ echo "Hello World" > index.html
```
4. Add, commit & push your "Hello World"! 
```
$ git add --all
$ git commit -m "Initial commit"
$ git push -u origin master
```
5. Head on to http://yourusername.github.io! You'll see a 'Hello World' printed on the page. 

### 2. Set up Jekyll
Jekyll is a static site generator. Simply put, static site generators takes the content we give it & outputs it on the screen. There's no dynamic interaction such as requesting for files etc. and is really useful for simple sites like a blog. If you're interested to know more, you can take a look [here](https://davidwalsh.name/introduction-static-site-generators).  
<br>
To install jekyll, do 
```
# Install Jekyll and Bundler gems through RubyGems
$ gem install jekyll bundler 

# Create a new Jekyll site at ./mysite
$ jekyll new mysite

# cd into our new directory
$ cd mysite

# Build the site & serve it. This will serve our site at localhost:4000. Hop on over to see the generated site! 
/mysite $ bundle exec jekyll serve 
```
Your newly generated site will look something like this: 
<figure>
<img class="jekylldir" src="/../images/jekyll_gensite.png" alt="Jekyll's Generated Site"/>
</figure>
<br>
After that's done, this is what you'll see in your directory:
```
$ ls
$ Gemfile, _config.yml, _site, index.md, Gemfile.lock, _posts, about.md
```
<br>
Jekyll follows a directory structure, such as the one shown below, so its good to get to know what's going on before we get started.  
<figure>
<img class="jekylldir" src="/../images/jekyll_dir.png" alt="Jekyll's Directory"/>
</figure>
<br>
Rough idea on the various directories:
+ _drafts: Contains posts which we do not want to post / show on our site yet
+ _includes: The partials that we can use in our layout
+ _layouts: Consists of the templates that wrap posts
+ _posts: The content of our page! Format has to be in: YEAR-MONTH-DAY-title.md/html/whateveryoulike
+ _sass: Sass partials that can be imported to main.scss while will be processed into main.css for our site
+ _site: Folder where our generated site will be stored! Don't touch this folder
+ index.html: Where our template for the index page goes 

Okay, now that we roughly know where our stuffs should go, its time to start building the structure. I didn't really like the template that jekyll had, so I created my own template & styles. The first thing we wanna do is to create the base template for our site. Soo let's go and create a _layouts folder and add a default.html inside. You can name the file anything you like!

In this file, write the template you want for your home page. I'll put a simple example below: 
```
<!DOCTYPE html>
<html>
<head>
	<title>Huiwen</title>
</head>
<body>
	<div class="main-container">
		{'{ content }} 
	</div>
</body>
</html>
```

Now, we have a default layout that we can use on every page! yay! What does this mean? It means we can now include this (which is also called the front matter):
```
---
layout: default
---
```
in our index.html and it will render the layout we had in _layouts/default.html. Whatever content that you put below the front matter will be rendered into the content tags that we wrote in _layouts/defaults.html. 

The next thing we can do is to show all the posts that we have written. A simple loop such as the one below will work: (Ignore the ' in the code. I had to do that to stop the code from running LOL) 

```
---
layout: default
---
<ul>
	{'% for post in site.posts %}
		<li>  
			<'a href="{'{ post.url }}">{'{ post.title }}</a>
		</li>           
	{'% endfor %}      
</ul>   
```
There're many other useful stuffs such as the link tag, which allows you to link to a post, page or file in your project! Aaaand that's about it! The [jekyll docs](https://jekyllrb.com/docs/usage/) were really useful in helping me set up my page.
