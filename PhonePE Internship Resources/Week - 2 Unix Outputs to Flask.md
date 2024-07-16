# Week - 2

### Task Set - 1

Week 2 would concentrate around Bash/Perl/Python/Docker and learning how to parse data.

The following is the task:

Logfile:

[https://raw.githubusercontent.com/aratik711/nginx-log-generator/main/access.log](https://raw.githubusercontent.com/aratik711/nginx-log-generator/main/access.log)

The format of the logs is as follows:ip, time, httpMethod, path, httpVersion, statusCode, responseTime, upstream_ip:port, bodyBytesSent, referrer, userAgent, ssl_protocol, content_type, host

- Write a bash script to part the logs and provide the stats mentioned below.
- Write a perl script to generate the above output in HTML where each Day is a collapsible button ( [https://www.w3schools.com/bootstrap/bootstrap_collapse.asp](https://www.w3schools.com/bootstrap/bootstrap_collapse.asp) ) and when clicked it shows the stats.
- Create a web UI using Python Flask where we upload an nginx log file and it creates a view like the above perl script .

### Task Set - 2 - Flask Web Page Setup

The stats on the web page/script should show the following:

**Summary for the day/week/month:**

- highest requested host
- highest requested upstream_ip
- highest requested path (upto 2 subdirectories ex: /check/balance)

 total requests per status code (Ex: count of requests returning 404/401/502/504/500/200)

Top requests

- top 5 requests by upstream_ip
- top 5 requests by host
- top 5 requests by bodyBytesSent
- top 5 requests by path (upto 2 subdirectories ex: /check/balance)
- top 5 requests with the highest response timeget
- top 5 requests returning 200/5xx/4xx per host

 find the time last 200/5xx/4xx was received for a particular host

 get all request for the last 10 minutes

get all the requests taking more than 2/5/10 secs to respond

get all the requests in the specified timestamp (UI should have the capability to accept to and from timestamp Ex: from: 06/Mar/2021:04:48 to 06/Mar/2021:04:58)

The app should also be able to take the host (Ex: [apptwo-new.ppops.com](http://apptwo-new.ppops.com/)) as input and give the stats mentioned above. 

### Additional Task

- Create 2 docker images for the above perl and python application. We should be able to access/use the application if the docker container is created from the image.
- Make sure to push all of your tasks to git.
