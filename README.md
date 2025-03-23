<div align="center">
  <a href="https://www.buildwithfern.com/?utm_source=github&utm_medium=readme&utm_campaign=docs-starter-openapi&utm_content=logo">
    <img src="/fern/docs/assets/fern-logo.png" height="50" alt="Fern Logo" />
  </a>
</div>

# Docs RBAC Demo

This website builds off of the [Docs Starter](https://github.com/fern-api/docs-starter) template and is a demo for the [RBAC](https://buildwithfern.com/learn/docs/building-and-customizing-your-docs/rbac) feature.

## Key Files

- `fern/docs.yml` - The configuration file for the docs that contains RBAC roles and viewer permissions

## Live Demo

View the site at [plantstore-rbac.ferndocs.app](https://plantstore-rbac.ferndocs.app)

## How to Configure RBAC for Your Own Docs

1. Define your roles in [`fern/docs.yml`](https://github.com/fern-demo/rbac-example/blob/22d72c53d4f397bbef70b3e1c69e2e27995a363f/fern/docs.yml#L5-L9)
2. Add to the navigation in [`fern/docs.yml`](https://github.com/fern-demo/rbac-example/blob/22d72c53d4f397bbef70b3e1c69e2e27995a363f/fern/docs.yml#L28-L31)
3. Set up auth (requires contacting [Fern support](https://buildwithfern.com/learn#get-support))