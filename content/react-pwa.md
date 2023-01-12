# React Create a PWA with TypeScript

Suppose you want to create a PWA from your React TypeScript project, what should you do if you didn't started with a PWA template provided by create-react-app or if you want to start the app using vite instead of create-react-app and you don't have a template for pwa.

I was in this situation and I followed the following steps before launching my pwa on netlify:

- Add favicon / app logo and other icons
- Add app manifest
- Add service workers
- Deploy to netlify

IF you want to check out my netlify demo link: [Gift List Demo App](https://willowy-moxie-f6e35f.netlify.app/) or if want to take a look at my repo: [Github repo](https://github.com/davidschinteie/giftlist)

## Add favicon / app logo and other icons

If you already have this images in your app, skip to the next step.

If not, you'll need a svg (ideally) or a large png image (512x512 for optimal results) with your app logo.

I used [this favicon generator](https://realfavicongenerator.net/) which is really useful for this sort of usecases.

After you download their generated files, you should move them into your `public` folder and add their markup code to `index.html` file at the root of your project.

```html
<!-- Favicon -->
<link rel="shortcut icon" href="favicon.ico" />
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png" />
<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png" />
<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png" />
<link rel="manifest" href="/manifest.json" />
<link rel="mask-icon" href="/safari-pinned-tab.svg" color="#eab308" />
<meta name="msapplication-TileColor" content="#eab308" />
<meta name="theme-color" content="#eab308" />
<!-- End Favicon -->
```

### Add maskable icon

Maskable icons are a new icon format that give you more control and let your Progressive Web App use adaptive icons. If you supply a maskable icon, your icon can fill up the entire shape and look great on all Android devices.

For this, you can use the [Maskable.app Editor](https://maskable.app/editor). Upload your icon, adjust the color and size, then export the image and save it in the same `public` folder with the rest of your generated images.

## Add app manifest

Create a manifest.json file with the following structure:

```json
{
  "name": "Name of your app",
  "short_name": "Name of your app",
  "icons": [
    {
      "src": "/maskable_icon.png",
      "sizes": "200x200",
      "type": "image/png",
      "purpose": "any maskable"
    },
    {
      "src": "/android-chrome-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/android-chrome-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    },
    {
      "src": "/android-chrome-256x256.png",
      "sizes": "256x256",
      "type": "image/png"
    },
    {
      "src": "/android-chrome-384x384.png",
      "sizes": "384x384",
      "type": "image/png"
    }
  ],
  "theme_color": "#eab308",
  "background_color": "#eab308",
  "display": "standalone",
  "scope": "/",
  "start_url": "willowy-moxie-f6e35f.netlify.app"
}
```

For each icon you'll provide the path to the images created at the previous step, for the theme and background color I'll leave it to you to decide, as for display, you have to select `standalone` so your web app will behave similar to a native one.

For the scope you'll need to specify the path to your homepage. For my case, it was `/`, but for you, it can be `/sign-in` or `/home` etc. As for the `start_url` you'll need to specify your deployed public path, which in this case was the netlify provided link.

## Add service workers

At this step I cheated a bit cause I didn't now for sure what should be added so I used the template provided by create-react-app. If you'll explore their template, you can add the following files to your own react app: `src/service-worker.ts` and `src/serviceWorkerRegistration.ts`

In addition, the following should be added at the end of your `index.tsx / main.tsx` file:

```ts
serviceWorkerRegistration.register();
```

Also, if you look at the cra-template in the `package.json` file, you'll notice some `workbox-` dependencies. Go ahead and add them in your `package.json` and after, run a `npm install` in your project.

```json
"dependencies": {
    // other dependencies
    "workbox-background-sync": "^6.5.4",
    "workbox-broadcast-update": "^6.5.4",
    "workbox-cacheable-response": "^6.5.4",
    "workbox-core": "^6.5.4",
    "workbox-expiration": "^6.5.4",
    "workbox-google-analytics": "^6.5.4",
    "workbox-navigation-preload": "^6.5.4",
    "workbox-precaching": "^6.5.4",
    "workbox-range-requests": "^6.5.4",
    "workbox-routing": "^6.5.4",
    "workbox-strategies": "^6.5.4",
    "workbox-streams": "^6.5.4"
},
```

That's it!!! Now if you build the project and deploy it, you'll have a pwa that can be installed on your android/ios/macos/windows device. Isn't that sweeeeet?

## Deploy to netlify

Deploying your web app is going to be specific to how your app is developed. I used a free Netlify account, which offers a nice and simple way to host static web apps (one that is only HTML, JavaScript, and CSS).

Also, don't forget to edit the start URL in the manifest with your public deployed web app.
