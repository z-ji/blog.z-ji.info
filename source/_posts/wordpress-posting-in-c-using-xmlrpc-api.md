title: "Wordpress Posting in C# using xmlrpc api"
tags:
  - wordpress
id: 47
categories:
  - code snippets
date: 2012-04-26 13:31:12
---

First of all, you should enable XML-RPC call on wordpress if you use wordpress 2.6 and later.
How to enable XML-RPC? Just log in to your blog with the admin account, and go to Settings, and then Writing.
Check the box next to “XML-RPC” on the Writing options page, and save the settings.
You can directly consume XML-RPC calls in C#, or you can use some wrappers out there. The most popular wrapper out there is JoeBlogs. The source code is on github: [https://github.com/alexjamesbrown/JoeBlogs](https://github.com/alexjamesbrown/JoeBlogs). Once you get the source and make a successful build, you are ready to post in less then 10 lines of code.
take the following as an example.
```c#
var wpWrapper = new WordPressWrapper("http://host/xmlrpc.php", "usr", "pwd");
var post = new Post()
	{
		Body = "this is a test post",
		Categories = new string[]{"category b","category a"},
		Tags = new string[]{"tag b","tag a"},
		Title ="test title",
	};
wpWrapper.NewPost(post, true);
```