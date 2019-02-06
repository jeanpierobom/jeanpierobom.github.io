---
layout: post
title:  "Project A - Tutorial for a Small React Native App"
categories: [project-a]
---

THIS IS YET A DRAFT (work in progress)

In Project A, I developed a small app based on a [tutorial](https://medium.com/react-native-training/react-native-youtube-replica-f378200d91f0) created by Marlon Decosta. This app is a YouTube replica and was developed using React Native.

Inspired by this project, I am now providing another tutorial to create a similar app with React Native. Instead of using YouTube, we are going to develop an app that will communicate with Vimeo, another popular platform for video sharing.

#### Creating a Vimeo Account and Register an App

The first step in this tutorial is to create an account on [Vimeo](https://vimeo.com/). This is necessary for getting the credentials needed in order to interact with the API. Vimeo has different types of plans, but the free subscription is fine for this project. 
After creating an account, the next step is to register an app on [Vimeo Developer page](https://developer.vimeo.com/apps).

The following image illustrates the app registration form and suggested values for the fields:   

<img style="width: 100%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-1.PNG" alt="Register an APP on Vimeo">

After registering the app, we are able to generate an access token that will be used when communicating with the API. In the app details page, select the option for generating an access token according to the image below:

<img style="width: 100%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-2.PNG" alt="Generate Access Token">

After filling the form and clicking on the "Generate" button, the access token will be displayed. Take note of the following information from the app details page:
-	Client ID
-	Client Secret
-	Access Token

This information will be used later when connecting our app with the Vimeo API.

#### Installing Android Studio

React Native can be used to create apps for IOS and Android. For the purpose of this tutorial, I am going to focus on the Android Platform. That being said, the next step is to install Android Studio in order to use its Android emulator.

The latest version of Android Studio can be downloaded on [https://developer.android.com/studio/](https://developer.android.com/studio/). After installing Android Studio, we can open the AVD Manager on Tools / AVD Manager to install a virtual device.

<img style="width: 100%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-3.PNG" alt="AVD Manager">

After that, we can run the Android emulator. More detailed information about this process can be found in the [original tutorial](https://medium.com/react-native-training/react-native-youtube-replica-f378200d91f0).
 
#### Installing NodeJS

The next step in this tutorial is to install NodeJS. When installing NodeJS, a package manager called npm is also installed on your computer. We are going to use npm on the next sections of this tutorial.

The Node JS installer can be downloaded on [https://nodejs.org/](https://nodejs.org/)

#### Installing React Native

From now we will use the command line. Open your favorite terminal and type the following command to install React Native:

```
npm install -g react-native-cli
```

In the next step we will create the project folder by typing:

```
react-native init vimeo_react_native
```

The command ```react-native init``` initializes the folder with some preexisting code. At this point, it is possible to run the project on the emulator. make sure that your emulator is running and type the following commands on the terminal:

```
cd vimeo_react_native/
react-native run-android
```

The app will be executed on the emulator and hopefully you will see the following screen:  

<img style="width: 50%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-4.PNG" alt="Android Emulator">

#### Project Setup

In this section, we are going to add more packages to our project and write some code in Javascript.

First of all, let’s install some packages using npm:

```
npm i vimeo
npm i node-libs-react-native
npm i react-native-level-fs
npm i asyncstorage-down
```

There are a few packages required in this project that are available in NodeJS, but are not automatically available in React Native. In order to solve this problem, let's create a file named rn-cli.config.js on the project folder. Use your favorite editor and add the following content to this file:

```
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
```

This code creates an alias for each module needed by the vimeo package. For example, when the http module is required, the stream-http module provided by the node-libs-react-native will be used.

Next, open the index.js file and write the following line at the beginning of the file. 

```
require('node-libs-react-native/globals');
```

Now we are going to add the source code to App.js, that is the main component in this project and was automatically created when we generated the project folder with the ```react-native init``` command. So open this file in your editor and replace its content:

```
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 *
 * @format
 * @flow
 */

import React, {Component} from 'react'
import {
  Image,
  TouchableHighlight,
  TouchableOpacity,
  ScrollView,
  StyleSheet,
  Text,
  View,
  Linking
} from 'react-native'

class App extends Component {

  constructor(props) {
    super(props)
    this.state = {
      data: []
    }
  }

  componentDidMount() {
    const clientId = '<client id>'
    const clientSecret = '<client secret>'
    const accessToken = '<access token>'
    const channel = '/channels/staffpicks/videos';
    const fields = 'uri,name,link,description,duration,created_time,modified_time,pictures';

    var Vimeo = require('vimeo').Vimeo;
    const vimeoClient = new Vimeo(clientId, clientSecret, accessToken);

    vimeoClient.request({
      path: channel,
      query: {
        page: 1,
        per_page: 6,
        fields: fields
      }
    }, (error, body, status_code, headers) => {
      if (error) {
        console.log('error: ' + error);
      } else {
        console.log('body: ' + body);
        console.log('body.data: ' + body.data);
        const items = []
        body.data.forEach(item => {
            items.push(item)
        })
        this.setState({
            data: items
        })
      }
     
      console.log('status code: ' + status_code);
      console.log('headers: ' + headers);
    });
  }

  render() {
    return (
      <View style={styles.container}>
        <ScrollView>
          <View style={styles.body}>
            {this.state.data.map((item, i) => 
              <TouchableHighlight
                key={i}
                onPress={() => Linking.openURL(item.link) }>
                <View style={styles.vids}>
                <Image
                    source={{uri: item.pictures.sizes[5].link}}
                    style={{width: 320, height: 180}}/>
                <View style={styles.vidItems}>
                  <Image
                    source={require('./images/vimeo.png')}
                    style={{width: 40, height: 40, borderRadius: 20, marginRight: 5}}/>
                  <Text style={styles.vidText}>{item.name}</Text>
                </View>
              </View>                
            </TouchableHighlight>    
            )}
          </View>
        </ScrollView>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  body: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    padding: 30,
  },
  vids: {
    paddingBottom: 30,
    width: 320,
    alignItems: 'center',
    backgroundColor: '#fff',
    justifyContent: 'center',
    borderBottomWidth: 0.6,
    borderColor: '#aaa',
  },
  vidItems: {
    flexDirection: 'row',
    alignItems: 'center',
    justifyContent: 'space-around',
    padding: 10,
  },
  vidText: {
    padding: 20,
    color: '#000',
  },
});

export default App;
```

In the ```componentDidMount``` method, we are using the Vimeo credentials to connect with the API and to retrieve a list of videos from a channel called  Staff Picks. The ```render``` method is responsible for creating the interface and displaying a thumbnail and title for each video.

When the user clicks on a thumbnail, the respective video is opened on the default web browser on the user device. One improvement could be to add the Vimeo player for the video to be opened directly in the app.

When you are adding the code to the App.js file, remember to replace the information for the variables clientId, clientSecret and accessToken.

Finally, let’s create an "image" folder and place the Vimeo icon file.

<img style="width: 100px" src="https://jeanpierobom.github.io/assets/images/vimeo.png" alt="Vimeo Logo">

You can download this image file [here](https://jeanpierobom.github.io/assets/images/vimeo.png).

#### Final Product

This image illustrates how to app look like after finishing this tutorial.

<img style="width: 50%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-5.PNG" alt="Final Product">


#### Github
You can download all code for this project in the Github repository: [https://github.com/jeanpierobom/vimeo_react_native](https://github.com/jeanpierobom/vimeo_react_native)

#### Resources

Tutorial: React Native YouTube Replica<br>
Author: Marlon Decosta<br>
[https://medium.com/react-native-training/react-native-youtube-replica-f378200d91f0](https://medium.com/react-native-training/react-native-youtube-replica-f378200d91f0)
