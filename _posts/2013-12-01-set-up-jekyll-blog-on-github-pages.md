---
layout: post
title: Set up a Jekyll blog on github pages with custom domain
category: software
tag: [markdown, github]
---

Jekyll is a simple plain-text based blog system that is really easy set up. It focuses on contents and requires no databases and no comment moderations. The best part is that I can host it on github pages. I can write my blogs in markdown and just push it to a github repository to publish it. 

Here is the steps to set it up with github pages. 

System environments:

- Ruby 2.0
- It might work on Windows but I had problems start the local jekyll server on Windows7. So I'd recommend to try this on MacOS or Linux. 

### Install Jekyll locally
This includes installing required gems and importing posts from a wordpress blog, which requires installing a few additional gems. 

     # this is the gem
     sudo gem install jekyll
     # install gems for importing wordpress blog
     sudo gem install jekyll-import --pre
     sudo gem install sequel
     sudo gem install hpricot

I use a minimalistic theme called [dbyll](http://jekyllthemes.org/themes/dbyll/). Instead of run `jekyll new c13s` to create the blog, I directly unzipped the dbyll zip file. 

  wget https://github.com/dbtek/dbyll/archive/master.zip -o dbyll.zip
  unzip dbyll.zip
  cd dbyll-master
  # import wordpress exports
  ruby -rubygems -e 'require "jekyll-import";
      JekyllImport::Importers::WordpressDotCom.run({
            "source" => "../wordpress.xml"
    })'

make a few modifications to `_config.yml` to change the settings, and `_includes/page.html` `_includes/header.html`, to remove the dbyll branding on the title of the home page. 

- `_config.yml`
Change settings according to documentation
- `_includes/page.html`
Replace `<h1>{{ page.title }}` with `{% if page.title != 'dbyll' %} {{ page.title }} {% endif %}`
- `_includes/header.html`
Replace `<title{{ page.title }}</title>` with
`<title>{% if page.title == 'dbyll' %} WenKu {% else %} {{ page.title }} {% endif %}</title>`

	   
Start jekyll local server to check if it works correctly. 

      jekyll serve  
      # open http://localhost:4000

### Publish to GitHub Pages

To publish the local Jekyll blog to GitHub pages, you need set up either github project pages or use your user (or organization) pages. However, if you need to set up a custom domain with the github pages, then you'll have to use pages.  Here I am using a user pages repository `c13s.github.io`. 

   git clone git@github.com:c13s/c13s.github.io.git
   cd c13s.github.io

   mv ../dbyll-master/* .

   # set up my subdomain
   touch CNAME ; echo "c13s.jiao.me" > CNAME
   git add .
   git commit -a -m 'blog initial import'
   git push origin master

It might take up to 10 mins for the page to be ready in public. When it's ready, it's at http://c13s.github.io

The last step is to create a new CNAME `c13s` for my domain and point it to `c13s.github.io`. After it's propagated over the internet, I can open http://c13s.github.io and see my Jekyll blog on GitHub pages. 


    