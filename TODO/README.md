# TODO

There are a number of other things I want to explore as I get time. This file will be a scratchpad of sorts, and will likely become wildly out of date from time to time.

Here are some of the other things I want to explore/work on through the examples:

  - **Automatic host-to-container DNS resolution/routing**: There are some interesting solutions to the problem of "how do I use friendly domain names that can be automatically/easily configured" with Docker containers. Some important background:
    - [Configure container DNS](https://docs.docker.com/engine/userguide/networking/default_network/configure-dns/)
    - Tools like [resolvable](https://github.com/gliderlabs/resolvable) and [docker-dnsmasq](https://github.com/jpillora/docker-dnsmasq).
    - Techniques like building your own [Nginx](https://www.digitalocean.com/community/questions/how-to-bind-multiple-domains-ports-80-and-443-to-docker-contained-applications?answer=18095), Varnish, or HAProxy-based reverse proxy for easier domain-to-container mappings.
    - [Amazee.io](https://docs.amazee.io/local_docker_development/local_docker_development.html)'s approach: use [dnsmasq](https://hub.docker.com/r/andyshinn/dnsmasq/), [haproxy](https://hub.docker.com/r/amazeeio/haproxy/), and [ssh-agent](https://hub.docker.com/r/amazeeio/ssh-agent/) to connect things together.
  - **Docker mounted volume performance**: See the canonical thread, [File access in mounted volumes extremely slow, CPU bound](https://forums.docker.com/t/file-access-in-mounted-volumes-extremely-slow-cpu-bound/8076). Basically, performance on the Mac (haven't tested other environments) is abysmal for apps that require reading/writing of many or large files. Here are some interim solutions until `osxfs` is up to snuff:
    - [docker-sync](http://docker-sync.io/)
    - [docker-sync alternatives](https://github.com/EugenMayer/docker-sync/wiki/Alternatives-to-docker-sync)
  - TODO
