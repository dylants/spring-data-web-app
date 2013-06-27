# Spring Data Web Application #

## Overview ##
An example application that displays how to utilize Spring Data with MySQL.  This app
also illustrates how to configure Spring without any XML configuration files, and utilizing
servlet 3.0's ability to not include a web.xml file for deploy.

## Technical Details ##
The application provides a REST controller around a Book entity, and after starting the
application, you can hit this URL to view the existing Books (performing a GET request):  
	http://localhost:8080/spring/books

To add a Book, send a POST request to that URL with the body similar to:
	{
	  "title": "The Elegant Universe: Superstrings, Hidden Dimensions, and the Quest for the Ultimate Theory",
	  "author": "Brian Greene"
	}

Then, viewing the original URL, performing the GET request will result in:

	[
		{
			id: 1,
			title: "The Elegant Universe: Superstrings, Hidden Dimensions, and the Quest for the Ultimate Theory",
			author: "Brian Greene",
			new: false
		}
	]