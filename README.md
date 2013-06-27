# Spring Data Web Application #

## Overview ##
An example application that displays how to utilize Spring Data with MySQL.  This app
also illustrates how to configure Spring without any XML configuration files, and utilizing
servlet 3.0's ability to not include a web.xml file for deploy.

## Technical Details ##
### Spring Data with REST ###
The application provides a REST controller around a Book entity, and after starting the
application, you can hit this URL to view the existing Books (performing a GET request):

	http://localhost:8080/spring/books

Initially no books will be displayed.  To add a Book, send a POST request to that URL with the body similar to:

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

The <code>BooksController</code> provides both the GET and POST endpoints.  These in turn utilize the
<code>BookRepository</code> interface, which extends Spring's <code>PagingAndSortingRepository</code>.
This provides us not only with CRUD operations, but also operations to retrieve the data in pages, and
sort the data returned.  The <code>BookRepository</code> points to the <code>Book</code>, which is a simply
POJO annotated with <code>@Entity</code>.  This bean is configured to write to the Book database table
with colums for each attribute of <code>Book</code>, all done automatically by Spring Data.

### XML-less Configuration ###
By taking advantage of Servlet 3.0, we're able to remove our web.xml and in place, provide a Spring
Configuration bean.  Our <code>WebApplicationInitializer</code> extends
<code>AbstractAnnotationConfigDispatcherServletInitializer</code>, which allows us to provide details
normally specified in a web.xml file (such as root configuration, servlet configuration, and servlet mappings).
The <code>WebApplicationInitializer</code> also includes two Spring Configuration beans, one for the
root context (<code>RootContextConfiguration</code>) and one for our web application's servlet context
(<code>ServletContextConfiguration</code>).

The <code>RootContextConfiguration</code> is mostly responsible for loading up our data layer, by
creating beans for our <code>DataSource</code> and <code>EntityManager</code>.  It also scans for any
additional beans needed for our data layer (including the <code>@Entity</code> beans and our repository).
The <code>ServletContextConfiguration</code>, by including the <code>@EnableWebMvc</code> annotation,
provides Spring MVC support to the beans defined within.  This configuration scans for beans annotated
with <code>@Controller</code>, which includes our <code>BooksController</code> class.

## Getting Started ##

Clone the repo, and either import it into Eclipse for deploy or build the WAR file.  Deploy the application
to an application server, and hit the books url at: http://localhost:8080/spring/books
