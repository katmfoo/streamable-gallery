# streamable-gallery

The website https://streamable.com lets a user upload short video clips and send the link for others to view. But they do not have any way of showing other people a collection of all the clips you have uploaded. This web application creates a gallery of clips that were uploaded by a specific user. It uses a small custom API endpoint ([streamable-gallery-api](https://github.com/pricheal/streamable-gallery-api)) to emulate the correct cookies being sent from the logged in users browser and return the same data to this web application, when that data would normally only be available to the specific browser that the user is logged in on.

### Live demo

This web app is being run live right now at http://patricheal.com/clips/.

### How to use

```bash
# Clone this repository
$ git clone https://github.com/pricheal/streamable-gallery.git

# Go into the repository
$ cd streamable-gallery-api

# Install dependencies
$ npm install

# Serve with hot reload at localhost:8080
$ npm run dev

# Build for production with minification
$ npm run build
```

### Built with

* **Vuejs** for the general front end framework
* **Axios** to make the request to [streamable-gallery-api](https://github.com/pricheal/streamable-gallery-api)
* **Momentjs** for date formatting

