# URL Shortener

This project is a simple URL shortener built with Django and Docker. 

## Features
- Shorten long URLs
- Set expiration dates for shortened URLs
- Enquire about the status of shortened URLs (e.g., access count, time left before expiration)
- Redirect to the original URL from the shortened version

## Technologies Used
- **Backend**: Django
- **Frontend**: HTML, CSS, JavaScript
- **Containerization**: Docker
- **Database**: PostgreSQL

## Installation Instructions
1. Install docker on your local machine.
   Start the docker in the background

2. Clone the repository:
   ```bash
   git clone https://github.com/username/url-shortener.git
   cd url-shortener
   cd django-url-shortener

   alternate way can be
    download the project folder in your local machine and extract it and in cmd navigate inside the folder "django-url-shortener"

3.Once inside the folder run the command
  "docker-compose build"
  
  "docker-compose up"

4.Navigate to http://localhost:8000/ in your browser to access the URL shortener interface.


How the Project Works
The backend of the project uses PostgreSQL to store the data. The database consists of five columns:

  long_url: The original URL provided by the user.
  tiny_url: The shortened URL generated.
  access_count: The number of times the tiny URL has been accessed.
  creation_time: The timestamp when the tiny URL was created.
  expiration_time: The time when the tiny URL will expire.
  There are three main functions in views.py that handle the requests:

  1. Shorten URL (shorten_url view)
	This function handles a POST request to shorten a URL.
	It validates whether the provided URL is a valid URL.
	If the URL is valid:
	It checks if the URL already exists in the database:
	If it exists and is expired, it deletes the old record and generates a new unique tiny URL.
	If it exists and is not expired, it returns a message saying the URL is already shortened, along with the existing tiny URL.
	If the URL is not in the database, it generates a new tiny URL and stores the record in the database with the creation and expiration times.
  2. Enquire URL (enquire_url view)
	This function handles a GET request to get details about a tiny URL.
	It checks if the provided tiny URL exists in the database:
	If the tiny URL is found and is not expired:
	It returns the original URL, the access count, and how long the URL will remain valid before it expires.
	If the tiny URL is expired, it deletes it from the database and returns a message saying no tiny URL exists.
	If the tiny URL is not found, it returns an error message indicating the URL does not exist.
  3. Visit URL (visit_url view)
	This function handles a GET request to visit the original URL by providing a tiny URL.
	It checks if the provided tiny URL exists in the database:
	If it exists and is not expired:
	It increments the access count.
	It then redirects the user to the original long URL.
	If the tiny URL is expired, it deletes it from the database and does not redirect.
	If the tiny URL is not found, it returns an error indicating the URL does not exist.



Implementation Notes
  This implementation successfully ensures the following:

  Unique Tiny URL: Each tiny URL generated is unique.
  Unique Long URL: Each long URL can only have one corresponding tiny URL at any given time.
  Expired URL Handling: URLs that are expired cannot be accessed. If a tiny URL is expired, it will be deleted from the database.
  Re-generation of Tiny URL: If an expired URL is requested, a new tiny URL can be generated for the same long URL.

***I have made sure to handle all possible cases to prevent users from accessing an expired URL or generating a shortened URL for an already valid one. The system checks for URL validity, expiration, and uniqueness to ensure that only active, non-expired URLs are accessible. If a URL is expired, it is properly removed from the database, and users are prevented from accessing it. Additionally, the system ensures that each URL is unique, and if the URL is already shortened, it prevents duplication.***
 

***Currently, there is a minor issue with redirecting the tiny URL, which results in a CORS error that can be observed in the network tab. While I have attempted to handle this issue, it hasn't been fully resolved yet. Due to time constraints, I was only able to allocate one day for this project, and as a result, I couldn't implement additional functionalities or fix this issue completely.
I appreciate your understanding***


  
