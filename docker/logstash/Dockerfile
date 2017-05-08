FROM docker.elastic.co/logstash/logstash:5.4.0
# https://github.com/elastic/logstash-docker

MAINTAINER marcz
# Add your logstash plugins setup here
# Example: RUN logstash-plugin install logstash-filter-json

CMD ["logstash -f /usr/share/logstash/pipeline/logstash.conf"]
