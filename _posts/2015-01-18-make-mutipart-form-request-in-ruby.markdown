---
layout: post
title:  "Make a mutipart form post with (core) ruby"
date:   2015-01-27 11:49:10
categories: http web email 
---

I could not find a post talking about this so to alleviate a few hours of pain for other developers here is an example

{% highlight ruby %}
require "net/http"

# Token used to terminate the file in the post body. Make sure it is not
# present in the file you're uploading.
BOUNDARY = "AaB03x"

# set the uri to post to
uri = URI('http://url.you.want/to/post/to')

# get a http object
http = Net::HTTP.new(uri.host, uri.port)

# build the request
request = Net::HTTP::Post.new uri


post_body =  <<-POSTBODY
--#{BOUNDARY}\r
Content-Disposition: form-data; name=\"name\"\r
\r
bob\r
--#{BOUNDARY}\r
Content-Disposition: form-data; name=\"email\"\r
\r
bob@cool_guy.com\r
--#{BOUNDARY}\r
Content-Disposition: form-data; name=\"file\"; filename=\"file_name.txt\"\r
Content-Type: text/plain\r
\r
the content in the file\r
--#{BOUNDARY}--
\r
POSTBODY
# --#{BOUNDARY}\r
# Content-Disposition: form-data; name=\"action\"\r
# \r
# Preview\r
# --#{BOUNDARY}\r
# Content-Disposition: form-data; name=\"filelocationuploaded\"\r
# \r
# C:\\Program Files\\Journyx\\jwt\\tmp\\ps\\enhimProject\\tmp\\tmpmij7tg\r


request.body = post_body
request["Content-Type"] = "multipart/form-data; boundary=#{BOUNDARY}"
#this sends the propper cookie
request["Cookie"] = "calendar_period_type=year; rui_searchopts_expanded=1; wtsession=#{WTSESSION}"


response = http.request request # Net::HTTPResponse object
puts response.body
# print post_body
{% endhighlight %}

[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll]:    http://jekyllrb.com
