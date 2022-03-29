---
layout: post
title: Automating the creation of posts in Jekyll
---

I like to automate things that are a bit tedious. So, I've automated a slightly tedious thing and probably put more time into than will pay off. But perhaps it might help someone else.

To make a post, you put a new file in the `_posts/` directory. This file must be named strictly in the form of ``"yyyy-mm-dd-post-title.md"``. Once the file is created, a header is required of the following form:

{% highlight bash %}
---
layout: post
title: Automating the Creation of Posts in Jekyll
---
{% endhighlight %}

This is all stuff that I want created for me -- the computer knows the date, right? Why do I have to type that in? And once I declare the title of the post once, why should I type that again? 

Obviously I'm just lazy. So, I wrote the ugliest Ruby code you've ever seen to automate the post-file creation. 

{% highlight ruby %}
#!/usr/bin/ruby

# introduce ourselves
def greet
  puts "** rywhite's Postmaker - a Jekyll blog post Generator **"
end


# get post title from user
def getPostTitle
  print "Post title: "
  title = gets
  return title
end

# construct file name
def buildFileName(title)
  fileTitle = title.downcase # convert all letters to lower case
  fileTitle = fileTitle.strip # remove all leading or trailing whitespace
  fileTitle = fileTitle.gsub(' ', '-') # convert spaces to dashes
  fileTitle = fileTitle.gsub(/[^\w-]/, '') # removes all non-alpha, non-dash characters, similar 
                                           # to \W, but includes the '-', so we spell it out here
  today = Time.new
  fileName = today.strftime("%Y-%m-%d") + "-" + fileTitle + ".md"
  puts "Post file: " + fileName
  return fileName
end

def toWriteFile(fileName)
  toWrite = true
  if File.file?("_posts/" + fileName)
    puts "This post file already exists. Do you want to overwrite it?"
    print "y/n: "
    input = gets
    toWrite = input.downcase == "y" ? true : false
  end
  
  return toWrite
end

# main script logic
title = getPostTitle
fileName = buildFileName(title)
toWrite = toWriteFile(fileName)

# construct file content
if toWrite
  fileContent = "---\nlayout: post\ntitle: " + title.strip + "\n---\n"
  puts fileContent

  # print post outline to new file. if file already exists, we've already 
  # gotten permission to overwrite it
  file = File.new("_posts/" + fileName, "w+")
  if file
    file.syswrite(fileContent)
  else
    puts "Unable to open file."
  end
end


puts "Done."
{% endhighlight %}

I placed the script in the `_posts/` directory, and added the file to my `.gitignore` so that Git wouldn't try to add it to the server. 

What does it look like in action?

![Postmaker in action](/images/postmaker_run.png)

I've never written anything in Ruby before, so this was a fun chance to learn the basics of the language. 

