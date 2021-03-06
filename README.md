# What is snowplow.github.com?

snowplow.github.com is the source code and content for [http://snowplowanalytics.com](http://snowplowanalytics.com) website.

The site is built in [Jekyll](https://github.com/mojombo/jekyll) and served by [Github Pages](http://pages.github.com/). (Thanks to the great people of Github for this fantastic service.) 

## Content covered

The content covers:

1. What Snowplow is, including why we developed Snowplow, and how Snowplow differs from other approaches to web analytics
2. How Snowplow works, from a technical standpoint
3. How we plan to develop Snowplow
4. How to perform analyses using Snowplow
5. Overview of the Professional Services we offer around Snowplow

All content is copyright (c) 2012 Snowplow Analytics Ltd and not to be reused without permission. 


## Adding new content and editing existing content

We welcome fixes and improvements! In particular on the analysis side, there is a wide range of ways Snowplow can be used, and we can't possibly hope to cover them all ourselves. If you've used it for a specific type of analysis, we'd love you to share it here. Please fork the repo, make your changes and then create a pull request.

### Contributing to the website

1. [Fork](https://help.github.com/articles/fork-a-repo) the repository
2. Clone your fork to your local machine
3. Make the desired changes to the webiste. More details on this are [below](#website-update)
4. Make a [pull request](https://help.github.com/articles/using-pull-requests). We will incorporate your updates to the site ASAP


<a name="website-update" />
### Updating the website

1. [A note about plugins and Github pages](#1-a-note-about-plugins-and-github-pages)
2. [Making changes locally](#2-making-changes-locally)
3. [Adding a new blog post](#3-adding-a-new-blog-post)
4. [Adding pages to the website (not the blog)](#4-adding-pages-to-the-website-not-the-blog)
5. [Embedding images](#5-embedding-images)
6. [Links between pages](#6-links-between-pages)
7. [Side menus](#7-side-menus)
8. [Previewing changes locally](#8-previewing-the-changes-locally)
9. [Committing your changes and deploying them to Github Pages](#9-committing-your-changes-and-deploying-them-to-github-pages)

#### 1. A note about plugins and Github pages

The Snowplow Analytics website uses Jekyll plugins for additional functionality. (E.g. pagination). These are **not** supported by Github pages. As a result, we have to use a workaround. (Kindly provided by [Alexandre Rademaker](http://arademaker.github.com/) [here](http://arademaker.github.com/blog/2011/12/01/github-pages-jekyll-plugins.html))

1. All source files for the website (e.g. markdown files etc.) are stored in the **source** branch (or in a dedicated branch just for that update e.g. "post/my-new-post")
2. Jekyll should be run locally to generate the static html files locally
3. These static files (saved in the `_site` folder) should be copied to the root directory on the master branch
4. These static files are the ones that are then served on Github pages
5. As a result, **does not** need to process the files in Github pages. That is why the `.nojekyll` file is included in the master branch

As all the Jekyll processing is performed locally, you need to make sure that Jekyll and all the plugins used are installed on your local machine. Because Jekyll runs on top of Ruby, you'll need a copy of Ruby locally, and then you'll need to install:

1. [jekyll](https://github.com/mojombo/jekyll). To install the gem execute `sudo gem install jekyll`.
2. [jekyll-pagination](https://github.com/blackwinter/jekyll-pagination). To install the gem execute `sudo gem install jekyll-pagination`.
3. [Pygments](http://pygments.org/). To install, `sudo apt-get install python-pygments`
4. [Sass](http://sass-lang.com/). (If you want to edit the CSS.) To install, `sudo gem install sass`. To use, edit `static/css/styles.scss` and then execute `sass static/css/styles.scss:static/css/styles.css` to generate the geniune CSS from the SCSS.

To make things easier, you can make use of our [dev-environment] [dev-environment], which can be provisioned with all the required dependencies. To set this up:

First: make sure you have both Virtual Box and Vagrant installed locally.

Secondly, clone the [dev-environment] [dev-enviornment]:

	$ git clone git@github.com:snowplow/dev-environment.git

Build the VM:

	$ cd dev-environment
	$ vagrant up

Once the basic box is provisioned, you need to run two ansible playbooks to install Ruby, RVM, Jekyll and the other required gems. First SSH into the VM:

	$ vagrant ssh

Then install Ruby via the playbook:

	$ ansible-playbook /vagrant/ansible-playbooks/ruby.yaml --inventory-file=/home/vagrant/ansible_hosts --connection=local

Exit the VM and then re-enter it. (This is necessary to switch on RVM and switch to using Ruby 1.9.3):

	$ exit
	$ vagrant ssh

Then install Jekyll and the required Jekyll plugins:

	$ ansible-playbook /vagrant/ansible-playbooks/jekyll.yaml --inventory-file=/home/vagrant/ansible_hosts --connection=local

Great! Now you have your development environment setup. Now you can download this repo. We recommend you do this in the host:

	$ exit
	$ git clone git@github.com:snowplow/snowplow.github.com.git

More explanation on how to update the site is given below:

#### 2. Making changes locally

In your host, switch to the source branch to update the source files

	git checkout source
	git pull 	# to ensure you're working on the latest version

Make the relevant changes to the site. To add a new blog post, for example, see the next section: 

#### 3. Adding a new blog post

This is straightforward. Create a new markdown file in the _posts directory, with the filename format `YYYY-MM--DD-title.md`.

In the file add the following [YAML Front Matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter) to the top of the post:

	---
	layout: blog-post
	shortenedlink: {{TITLE YOU WANT TO APPEAR ON THE BLOG SIDEBAR}}
	title: {{FULL TITLE DISPLAYED AT THE TOP OF THE POST}}
	tags: {{ANY TAGS}}
	---

Then type in the rest of the post, in markdown, as normal.

#### 4. Adding pages to the website (not the blog)

Adding a page to the rest of the site is reasonably straightforward.

First, create an appropriately titled markdown file and save it to the appropriate directory. (The site folder structure reflects the site information structure, so if you want to create a new page in the 'Product' section it's best to save it in the product folder.)

Second, add a [YAML Front Matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter) to the top of the Markdown file e.g.:

	---
	layout: section
	category: {{e.g. 'product', 'services', 'analytics', 'contact'}}
	title: {{PAGE TITLE (used in side menu)}} 
	weight: {{INTEGER INDICATING WHERE ON THE SIDE MENU THIS SHOULD APPEAR i.e. 1 = top spot}}
	---

Note that if you're adding content to the Analytics section, you'll need to specify the 'analytics_category' you want the page to belong to e.g. 'product', 'catalogue' etc, as pages are currently divided into these subsections

#### 5. Embedding images

Images are committed to the appropriate /img folder (depending on which section the image belongs). It can then be referenced in the Markdown as normal.
	
#### 6. Links between pages

When adding links to between pages on the site:

1. Always use relative URLs e.g. `/analytics/catalogue-analytics/overview.html`
2. Remember all URLs end in `.html`

#### 7. Side menus

Side menus should automatically update with new blog posts / web pages as appropriate.

When the side menu is generated (Jekyll compiles the site) it fetches all the different pages, filters them by category and uses this to populate all the menus. To see how this is done, refer to the `_includes/sidebar_analytics`, `_sidebar_contact.html` etc. files. (There is one file for each side menu.)

#### 8. Previewing the changes locally

This is advisable before performing any commits :-). To preview the site on your host machine, you'll need to build it in the VM (guest machine):

	$ vagrant ssh
	$ cd /vagrant/snowplow.github.com
	$ jekyll --server

This will build the website and serve it to port localhost:4000. The Vagrant box is setup to forward this to localhost:4001 on your host machine, so open up a browser and enter `localhost:4001`. You should see the updated website displayed.

Remember, if you don't like what you see, and modify the site, to preview the changes, you'll need to CTRL-C out of Jekyll, then re-run 

	jekyll --server

from the repo directory, (which will recompile it).

#### 9. Committing your changes and deploying them to Github Pages

Once you are happy with the updates you've made to the website, you need to deploy them to production.

Remember: source files are stored in the `source` branch. (Which you have been editting.) To deploy, we need to get Jekyll to generate the static html pages, copy them to our master repo and then push them to Github. To do this:

Firstly add and commit the changes made to the source repo. Do this in the host (rather than the guest VM)

	$ exit
	// make relevant changes and updates to the site
	$ git add .
	$ git commit -m "{{enter description of changes made}}"

Now switch to the master branch

	git checkout master

Now we want to copy the static html files for the website into the root folder of the master branch. These will be the pages served by Github Pages. (Remember - we also add the `.nojekyll` file so Github pages doesn't use Jekyll to do any processing after we've finished the upload.) On Ubuntu / OSX:

	cp -r _site/* . && rm -rf _site/

On Windows:

	cp _site/* -recurse ./
	remove-item _site/* -recurse

Now add and commit your changes:

    git add . && git commit -a

And finally push both the source and master branches to origin

	git push --all origin

All done! You should see the updates pushed to the live site in a matter of minutes. (You may need to clear the browser cache to see them.)

[dev-environment]: https://github.com/snowplow/dev-environment