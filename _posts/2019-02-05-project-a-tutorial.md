---
layout: post
title:  "Project A - Tutorial"
categories: [project-a]
---

THIS IS YET A DRAFT (work in progress)

In Project A, I developed a small app based on a [tutorial](https://medium.com/react-native-training/react-native-youtube-replica-f378200d91f0) created by Marlon Decosta. This app is a YouTube replica and was developed using React Native.

Inspired on this project, I am now providing another tutorial to create a similar app with same technology. This time, we are going to develop an app that will communicate with Vimeo, another popular platform for video sharing.

#### Create a Vimeo Account and Register an APP

The first step in this tutorial is to create an account on [Vimeo](https://vimeo.com/). The free subscription is fine for this project. 
After creating an account, the next step is to register an app on [Vimeo Developer page](https://developer.vimeo.com/apps).

<img style="width: 50%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-1.PNG" alt="Register an APP on Vimeo">

After registering the app, we are able to generate an access token that will be used when communicating with the API.

<img style="width: 50%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-2.PNG" alt="Generate Access Token">

From the app details page, we will need to take note of the following information:
-	Client ID
-	Client Secret
-	Access Token

#### Install Android Studio

React Native can be used to create apps for IOS and Android. For the purpose of this tutorial, I am going to focus on the Android Platform. That being said, the next step is to install Android Studio in order to use its Android emulator.

The latest version of Android Studio can be downloaded here: https://developer.android.com/studio/. After installing Android Studio, we can open the AVD Manager on Tools / AVD Manager to install a virtual device.

<img style="width: 50%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-3.PNG" alt="AVD Manager">

After that, we can run the Android emulator. More detailed information about this can be found in the original tutorial.
 
#### Install NodeJS

The next step in this tutorial is to install NodeJS. The Node JS installer can be downloaded on https://nodejs.org/. When installing NodeJS, a package manager called NPM is also installed.

#### Install React Native

From now we will use the command line. Open your favorite terminal and type the following command to install React Native:

npm install -g react-native-cli

In the next step we will create the project folder by typing:

react-native init vimeo_react_native

At this point it is possible to run the project on the emulator.

cd vimeo_react_native/
react-native run-android

<img style="width: 50%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-4.PNG" alt="Android Emulator">

The command react-native init initializes the folder with some preexisting code.

#### Project Setup

The command react-native init initializes the folder with some preexisting code. In this section we are going to add more packages to the project and write some code in Javascript.

Let’s install a few packages and write some code in Javascript.

npm install vimeo
npm i node-libs-react-native
npm i react-native-level-fs
npm i asyncstorage-down
npm i react-native-vector-icons

Create a file called rn-cli.config.js on the root folder:

module.exports = {
  resolver: {
    extraNodeModules: {
      http: require.resolve('stream-http'),
      https: require.resolve('https-browserify'),
      fs: require.resolve('react-native-level-fs'),
      path: require.resolve('path-browserify'),
      stream: require.resolve('readable-stream')
    }
  }
};


<img style="width: 50%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-5.PNG" alt="Final Product">


#### Resources

Tutorial: React Native YouTube Replica<br>
Author: Marlon Decosta<br>
[https://medium.com/react-native-training/react-native-youtube-replica-f378200d91f0](https://medium.com/react-native-training/react-native-youtube-replica-f378200d91f0)
