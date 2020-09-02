# netlify-cms-oauth-provider-go

Netlify-CMS OAuth client sending token in form as Netlify service itself, implementation in Go (golang).

## Installation

Compile from source

- Download source & navigate to directory
- `go get` to fetch dependencies
- `go build main.go` to build

or,

- TODO: fix makefile

Download binary release

- TODO

## Configuration

### Auth Provider

Configuration is done with environment variables, which can be supplied as command line arguments, added in your app hosting interface, or loaded from a .env ([dotenv](https://github.com/motdotla/dotenv)) file.

Example .env file:

```
HOST=localhost:3000
BASE=https://your.server.com/auth
SESSION_SECRET=random-uuid
GITHUB_KEY=enter-github-client-id
GITHUB_SECRET=enter-github-client-secret
BITBUCKET_KEY=
BITBUCKET_SECRET=
GITLAB_KEY=
GITLAB_SECRET=
```

_Host_: The OAuth provider host & port. (default `localhost:3000`)

_Base_: The URL where the OAuth provider can be accessed publicly. This **must** match the _Authorization callback URL_ configured on GitHub etc. This is required when accessing the OAuth provider via a reverse proxy in order to allow the _Authorization callback URL_ to match. (default _HOST_).

*Session Secret:* Random string. (Can use a random uuid).

*Client ID & Client Secret:* After registering your OAuth app you will be able to get your client id and client secret.

- [GitHub OAuth Apps](https://github.com/settings/developers)

### CMS

You need to add `base_url` & `auth_endpoint` to the backend section of your netlify-cms config file. `base_url` is the live URL of this repo with no trailing slashes (just the base domain, no path). `auth_endpoint` is the path to append to _base_url_ for authentication requests. (In the example below, `/auth/auth` the first occurence of `/auth` is a subdirectory used to reach the OAuth provider and should be set to whatever path you choose, the second `/auth` is required to call the auth component of this provider). [Netlify CMS Backend Config](https://www.netlifycms.org/docs/backends-overview/).

```
backend:
  name: github
  repo: user/repo   # Path to your Github repository
  branch: master    # Branch to update
  base_url: https://your.server.com # Path to ext auth provider (just the base domain, no path)
  auth_endpoint: /auth/auth # Path to append to base_url for authentication requests (i.e. https://your.server.com/auth/auth)
```

### Reverse proxy

If you have a web server such as [NGINX](https://www.nginx.com) already running, you can add a location block to the relevant [server block](https://www.nginx.com/resources/wiki/start/topics/examples/server_blocks/) within the your.site.com.conf.

```
location /auth/ {
  proxy_pass http://127.0.0.1:3000/;
}
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## Authors and acknowledgment

Origianlly written by [Igor Kuznetsov](https://github.com/igk1972/netlify-cms-oauth-provider-go), and inspired by [netlify-cms-github-oauth-provider](https://github.com/vencax/netlify-cms-github-oauth-provider) (node-js). Thanks Václav & Igor!

Further updates relating to SESSION_SECRET and reverse proxy support were provided by [Phil Ashby](https://github.com/phlash/netlify-cms-oauth-provider-go/commit/c06c73cfef309d29d12cdf32c92b85128374750d). Thanks Phil!

## License

Whilst Igor doesn't show a license, his repo is linked from [Netlify CMS](https://www.netlifycms.org/docs/external-oauth-clients/).
