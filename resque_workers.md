resque runner:
````bash
env TERM_CHILD=1 INTERVAL=1 RESQUE_TERM_TIMEOUT=10 VERBOSE=true QUEUE=* bundle exec rake resque:work
````
