# Deployment

Many workflows currently trigger GitHub Actions and Pages deployment on every commit.
While the build doesn't take long, Pages can take a few minutes to transfer assets and deploy them.

What if a project runs builds locally and deploys to DO with Fabric.
This removes the dependency on Microsoft cloud services as anything more than a code repository.
Netlify CMS may compicate this. 

- Netlify (service dependent, teaching platform-specific products)
    + quick domain and certs, CDN, CMS, forms, CLI, atomic deploys, global CDN/deploys
    - AWS, dependent on external infrastructure, costs can change
- Digital Ocean (service independent, teaching core tech)
    + local, simple, OS control and maintenance, single region, simple, fixed costs 
    - need to update os, certs, maintain at least one droplet for static sites