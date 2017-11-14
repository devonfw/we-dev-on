# Run your Angular OASP4JS application as a PWA

The following steps will let you run your current application as a PWA. There is much more room for improvement, but as an initial approach is quite easy to make and test. 
 
## What is PWA?

![alt text](img/pwa_logo.png "PWA")

PWA or _Progressive Web Apps_ is a new standard in web development. According to [Google](https://developers.google.com/web/progressive-web-apps/):

Progressive Web Apps are user experiences that have the reach of the web, and are:

- **Reliable** - Load instantly and never show the [downasaur](https://www.google.com/search?q=downasaur&source=lnms&tbm=isch&sa=X&ved=0ahUKEwiTkvz3h77XAhVHIMAKHcFWCmUQ_AUICigB&biw=1536&bih=734), even in uncertain network conditions.
- **Fast** - Respond quickly to user interactions with silky smooth animations and no janky scrolling.
- **Engaging** - Feel like a natural app on the device, with an immersive user experience.

Apart from these characteristics, PWAs are usually responsive and should adopt to most devices and screen sizes. They are installable on mobile devices, meaning you can have a link on your home screen to launch the app right away making it *easier to access*. And they work in all web browsers. In modern browsers, we will leverage the amazing APIs we will talk about later, but in older browsers, our app should still work perfectly fine, without the amazing modern stuff of course.

## Instructions

We suppose that we have a recent **Angular 4+** application that works fine. This application was initially created using the [Angular CLI](https://cli.angular.io/) following the latest **OASP4JS instructions** in our tutorials or reference applications. 

We will need the following key elements to turn a standard web application into a PWA:

- Service Worker
- Service Worker Manifest
- App Manifest

### Making the app reliable & faster

We want our application to be reliable. It should work even if the network is poor or is unavailable. We will be using Service Workers to cache our application and avoid poor network connections issues. 

To achieve this, we can configure the service worker manually. I recommend using the pwa tools that the Angular team has provided. We are going to install those tools now to introduce a service worker in our application.

```bash
$ npm install --save @angular/service-worker
# or if you use yarn
$ yarn add @angular/service-worker
```
Now we will enable **service workers** inside our application. By default, they’re turned off in angular-cli when creating a project. We can enable them by executing the following command from project root which updates our `.angular-cli.json`:

```bash
$ ng set apps.0.serviceWorker=true 
```

> A service worker is a script that your browser runs in the background, separate from a web page, opening the door to features that don’t need a web page or user interaction. Today, they already include features like push notifications and background sync. — [Matt Gaunt](https://developers.google.com/web/fundamentals/getting-started/primers/service-workers)

Now if we build our application with `ng build --prod` the Angular CLI will include the following files in the `/dist` folder:

- **scripts.xxxxx.bundle.js** with the service worker installation script.  
- **worker-basic.min.js** is the service worker initiator.
- **ngsw-manifest.json** is the manifest file with the information with all the internal and external assets that are going to be cached. 

To test it we can install globally a small http-server and run it inside our dist folder as the `ng serve` command is not compatible with the PWA workflow:

```bash
$ npm install -g http-server 
$ cd dist
$ http-server
```

Now if you browse http://localhost:8080 in Chrome you will see your application running as a PWA.

We can always test the performance of our PWA using [Lighthouse](https://developers.google.com/web/tools/lighthouse/) and improve areas of our application lacking in performance.

### Future Improvements

Of course, as I stated at the very beginning of this article this is only an initial approach. There are a lot of improvements to do in the future (many of them addressed already in the next Angular and Angular CLI versions):

- Include the new NGSW tool.
- Improve the manifest file. 
- Include routing information.
- Cache REST requests.
- Push notifications. 
- And many more...

For example, with **Angular 5** and the next **Angular CLI 1.6** we will be able to start from scratch creating our apps including many of the improvements mentioned as it follows:

```bash
$ ng new myAmazingPWA --service-worker
```

