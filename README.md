# docker playground
 useful commands


### PRUNE
- docker system --help
  - df: shows docker space usage
- docker image prune
- docker system prune (will delete everything that's not running)
- The big one is usually docker image prune -a which will remove all images you're not using. Use docker system df to see space usage.
- docker container prune
