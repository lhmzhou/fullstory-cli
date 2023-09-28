# jomolhari

A user analytics CLI that pulls user data from FullStory data export feature.

## Usage

You can use the CLI interactively or by passing command line arguments.

To get started, let's install the package:
`npm i jomolhari -g`.

Next, we need to create a `story.js` file in the root of our project. The `story.js` allows us to configure the CLI with the various options:

- `apiKey`: This is the API key you get out of FullStory. This is required.
- `routes`: This is a array of `{ route: string, page: string }` that
we can normalize our routes to. The routes table uses [path-to-regexp](https://www.npmjs.com/package/path-to-regexp)
under the hood to allow you to normalize complex URLs to human readable ones.
- `transforms`: This is a key/value map of properties we can use to transform data attributes.
- `blacklists`: This is a key/value map of properties that we want to exclude certain values from.

An example `story.js` might look like:

```js
module.exports = {
  "apiKey": "XXXXXXXXXXXXXXXXXXXXX",
  "blacklists": {
    "Customer": [
      "sandbox",
    ],
    "UserEmailDomain": [
      'user.com'
    ]
  },
  "transforms": {
    "Customer": (data) => {
      return data.PageUrlOrigin.replace('https://', '')
    }
  },
  "routes": [
    { "route": "/dashboard", "page": "/dashboard" },
    { "route": "/users/:id", "page": "/users/details" },
    { "route": "/users/:id/:tab?", "page": "/users/details" }
  ]
};
```

Now that everything is configured, we can run our CLI. The CLI can be ran
interactively which allows a user to type in values dynamically or via
command line arguments.

To run the CLI interactively, you can do `sbc` or if you want
to run it your command line arguments like: `sbc --offset=1 --type=JSON --aggregate=true`.
