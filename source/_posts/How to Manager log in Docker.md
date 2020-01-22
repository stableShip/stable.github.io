---

title: How to Manager log in Docker

date: 2019-04-15 10:19:32

tags: [Docker]

---

## soft link to STDOUT, STDERR to collect log

  

`RUN ln -sf /dev/stdout /var/log/nginx/access.log`

  

## mount bind space to collect log

`--mount type=bind,src=/opt/logs, dst=/usr/local/tomcat/logs/`

  

## mount volume to collect log

  

`--mount type=volume src=volume_name dst=/use/local/tomcat/logs/`

  

## use redis/mq to collect log

`docker→ redis/mq→ logstash → elasticsearch`