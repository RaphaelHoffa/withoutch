What I changed and why:

I started by modifying the Dockerfile to adopt a multi-stage build pattern (build and runtime stages). I used a more versatile image with a `-dev` suffix for the build stage and a "distroless" image for the runtime stage.
I removed any existing `apt-get` commands and configured `pip` to use the Chainguard `index-url`.
I performed the entire process on my personal computer (Windows). I had to adjust the version of a library (`psycopg2-binary`) because the specified version (2.9.9) could not be found in Chainguard's secure repositories, so I performed a minor update to v2.9.10.
I moved all secrets and sensitive data to environment variables in a `.env` file (and added it to `.gitignore`).
I also modified the entrypoint; since we are using a distroless image for the runtime environment—which lacks a shell or execution tools to minimize the container's attack surface—the original entrypoint script was no longer viable.
Adjustments were also required in the `docker-compose` file to switch from plain-text environment variables to hidden/secure environment variables.
Finally, I edited the Python script to replace the hardcoded database connection URL (containing the username and password) with environment variables.

The `.env.example` file is no longer needed.
The `entrypoint.sh` file is no longer needed.

In a real production environment, we wouldn't authenticate directly with `pip` inside the Dockerfile; instead, we would use a corporate registry or a similar solution. We would also use a corporate artifact repository for the images.


Which risks were reduced:
Implementing a distroless image in production significantly reduces the attack surface, and adopting Chainguard Libs drastically cuts the number of CVEs in open-source libraries, thereby hardening the environment. By default, Chainguard images do not run as root, but I also added a 'USER nonroot' line.

I really enjoyed the exercise; it provided a clear understanding of the concepts behind these two Chainguard products.


Regarding the use of AI in the exercise:

I relied heavily on official Chainguard documentation for certain concepts (such as multi-stage builds and Wolfi images) and used it to troubleshoot issues with environment variables—though, in most cases, the problems turned out to be related to local caching on my machine. It also helped with the syntax of certain commands that weren't fresh in my memory—such as 'docker compose build --no-cache' itself, which was frequently used during the exercise. I didn't used AI for code generating.
