---
layout: blogpost
title: Academic Website with ≤ 70 Lines of Python
date: 2014-06-09
tags: web python bibtex flask recursion
---

To get a bit more accustomed with web development in Python, I started trying out [Flask][4][^1], a 'micro-framework' for web development.

My previous academic homepage had a publication list backed by a CMS, so I thought porting everything to a stand-alone website would be a little cumbersome -- but it wasn't.

My requirements for the publication list were simple:

* Should be defined in a single [BibTex][6] file
* Sort and group references by year
* Link to preprints and final versions
* Display abstract
* Display a BibTex entry

Apart from that, I also wanted to have some simple way to blog, so I followed [this nice tutorial][2] on how to set up a mark-down based blog with Flask.[^2]

Then, I just grabbed Python's [bibtexparser][7] package and was good to go:

{% highlight python %}
	#!python
	from flask import Flask
	from flask import render_template
	from bibtexparser.bparser import BibTexParser
	from bibtexparser.customization import convert_to_unicode
	from collections import defaultdict
	from flatpages import FlatPages, pygments_style_defs
	
	import os
	import sys
	reload(sys)
	sys.setdefaultencoding('utf-8')
	
	#Configuration for blog:
	FLATPAGES_AUTO_RELOAD = True
	FLATPAGES_EXTENSION = '.md'
	FLATPAGES_ROOT = 'static'
	POST_DIR = 'posts'
	
	app = Flask(__name__)
	flatpages = FlatPages(app)
	app.config.from_object(__name__)
	
	@app.route('/')
	def about():
	    return render_template('about.html', nav='about')
	
	@app.route('/blog')
	def blog():
	    posts = [p for p in flatpages if p.path.startswith(POST_DIR)]
	    posts.sort(key=lambda item:item['date'], reverse=True)
	    return render_template('blog.html', nav='blog', posts=posts)
	
	@app.route('/blog/<name>/')
	def post(name):
	    path = '{}/{}'.format(POST_DIR, name)
	    post = flatpages.get_or_404(path)
	    return render_template('blogpost.html', nav='blog', post=post)
	
	@app.route('/publications')
	def publications():
	    #Load file
	    with open('static/refs.bib', 'r') as bibfile:
	        bp = BibTexParser(bibfile)
	        references = bp.get_entry_list()
	
	    # Order references by year
	    references.sort(key=lambda x: x['year'], reverse=True)        
	    refs = defaultdict(list)
	
	    #Preprocess the references
	    for r in references:
	        if 'labels' in r:
	            r['keywordlist'] = r['labels'].split(',')        
	        if 'booktitle' in r:
	            r['booktitle'] = r['booktitle'].replace('\&', '&amp;')
	        r['title'] = r['title'].replace('\&', '&amp;')
	        refs[r['year']].append(r)     
	
	    #Sort years
	    refsbyyear = []
	    for year in refs.keys():
	        refsbyyear.append((year,refs[year]))    
	    refsbyyear.sort(key=lambda x: x[0], reverse=True)    	
	    return render_template('publications.html', nav='publications', references = refsbyyear)

	if __name__ == '__main__':
   		app.run(debug=(os.environ.get('FLASK_DEBUG_MODE')=='TRUE'))
{% endhighlight %}

Apart from dealing with character escaping in BibTex (see line 55; this works for me but should be generalized) and the sorting of references, everything worked out of the box. 

I could imagine that this setup scales nicely to large BibTex files and even whole research groups, as you can [generate static websites from a Flask app][2] (didn't try that yet). It seems particularly useful if everyone in the group is already working with BibTex (and who doesn't? ;-).

Since BibTex is happy with custom fields, it is also quite easy to integrate additional information that is only relevant for the website. For example, I am using a `preprint` field for paths to preprint files. The bibtexparser picks those fields up automatically. 

A [Jinja][8] template for the publications list could look like this:

{% highlight html %}
{% raw %}
	{% extends "layout.html" %} 
	{% block content %} 
	{% for year, yearrefs in references %}
	<h3>{{year}}</h3>
	<hr/>
	<ul>
	    {% for ref in yearrefs %}
	    <li class="publicationItem">
	        {% autoescape off %} {{ref.author}} :
	        <br/>
	        <em>{{ref.title}}</em>.
	        <br/>
	        {% if ref.booktitle%} {{ref.booktitle}} {% endif %} 
	        {% if ref.journal%} {{ref.journal}} {{ref.volume}} {% if ref.number%} ({{ref.number}}) {% endif %} {% endif %} 
	        {% endautoescape %} 
	        {% if ref.keywordlist%}
	        	{% for kw in ref.keywordlist %}
	        	<span class="label">{{kw}}</span>
	        	{% endfor %} 
	        {% endif %} 
	        {% if ref.note %}
	        	<span class="label label-primary">{{ref.note}}</span>
	        {% endif %}
	        <br/>
	        {% if ref.preprint %}
	        	<a href="{{ url_for('static', filename=ref.preprint)}}" target="_blank">[preprint]</a>
	        {% endif %} 
	        {% if ref.preprintonline %}
	        	<a href="{{ ref.preprintonline }}" target="_blank">[preprint]</a>
	        {% endif %} 
	        {% if ref.slides %}
	        	<a href="{{ url_for('static', filename=ref.slides)}}" target="_blank">[talk slides]</a>
	        {% endif %} 
	        {% if ref.online %}
	        	<a href="{{ref.online}}" target="_blank">[view online]</a>
	        {% endif %} 
	        {% if ref.abstractonline %}
	        	<a href="{{ref.abstractonline}}" target="_blank">[view full abstract online]</a>
	        {% endif %} 
	        {% if ref.abstract %} 
	        	<a href="javascript:void(0)" onClick="$('\#abstractModal{{ref.id}}').modal({show: true});">[abstract]</a> 
	        	<!-- The abstract modal just contains the abstract text. -->
	        {% endif %}
	        <a href="javascript:void(0)" onClick="$('\#bibtexModal{{ref.id}}').modal({show: true});">[bibtex]</a>
	    	<!-- The BibTex modal constructs a valid entry from all 'important' fields above. -->
	    </li>
	    {% endfor %}
	</ul>
	{% endfor %} 
	{% endblock %}
{% endraw %}
{% endhighlight %}

So, Flask is indeed as 'micro' as possible, at least when it comes to simple websites... :-)

[^1]: Of course I gave [Django][3] a spin as well, but I don't really need most features and thus decided to [keep it simple][5].

[^2]: I also followed [this nice tutorial (German)][1] on how to configure Flask for my current ISP (Strato).


[1]: http://blog.goodcleanfun.de/2013/12/17/strato-powerweb-flask-webframework-einrichten/
[2]: http://www.jamesharding.ca/posts/simple-static-markdown-blog-in-flask/
[3]: https://www.djangoproject.com/
[4]: http://flask.pocoo.org/
[5]: http://en.wikipedia.org/wiki/KISS_principle
[6]: http://www.bibtex.org/
[7]: https://pypi.python.org/pypi/bibtexparser/0.5.5
[8]: http://jinja.pocoo.org/