# This is the repo for hosting up my blog

[![Netlify Status](https://api.netlify.com/api/v1/badges/3ad983c6-0f65-45eb-8205-3a0a54562891/deploy-status)](https://app.netlify.com/sites/hengdashi/deploys)

## Editable files

```text
.
└── assets
    └── img # profile image
└── config
    └── _default
        └── ... # configuration files
└── content
    └── articles
        └── ... # articles
└── layouts
    └── partials
        └── comments.html # comment section api
└── static
    └── ... # favicons
```

## Local Debugging

Run the following command in the project directory.
```bash
hugo server
```

## Host & Deploy

Create a MR and merge.
Netlify will follow the instruction provided in `netlify.toml` to build the website.

## Upgrade the theme

```bash
hugo mod get -u
```
