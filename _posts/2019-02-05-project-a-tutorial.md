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

<img style="width: 100%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-1.PNG" alt="Register an APP on Vimeo">

After registering the app, we are able to generate an access token that will be used when communicating with the API.

<img style="width: 100%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-2.PNG" alt="Generate Access Token">

From the app details page, we will need to take note of the following information:
-	Client ID
-	Client Secret
-	Access Token

#### Install Android Studio

React Native can be used to create apps for IOS and Android. For the purpose of this tutorial, I am going to focus on the Android Platform. That being said, the next step is to install Android Studio in order to use its Android emulator.

The latest version of Android Studio can be downloaded here: https://developer.android.com/studio/. After installing Android Studio, we can open the AVD Manager on Tools / AVD Manager to install a virtual device.

<img style="width: 100%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-3.PNG" alt="AVD Manager">

After that, we can run the Android emulator. More detailed information about this can be found in the original tutorial.
 
#### Install NodeJS

The next step in this tutorial is to install NodeJS. The Node JS installer can be downloaded on https://nodejs.org/. When installing NodeJS, a package manager called NPM is also installed.

#### Install React Native

From now we will use the command line. Open your favorite terminal and type the following command to install React Native:

```
npm install -g react-native-cli
```

In the next step we will create the project folder by typing:

```
react-native init vimeo_react_native
```

At this point it is possible to run the project on the emulator.

```
cd vimeo_react_native/
react-native run-android
```

<img style="width: 50%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-4.PNG" alt="Android Emulator">

The command react-native init initializes the folder with some preexisting code.

#### Project Setup

The command react-native init initializes the folder with some preexisting code. In this section we are going to add more packages to the project and write some code in Javascript.

Let’s install a few packages and write some code in Javascript.

```
npm install vimeo
npm i node-libs-react-native
npm i react-native-level-fs
npm i asyncstorage-down
npm i react-native-vector-icons
```

There are a few packages required in this project that are from NodeJS, but are not automatically available in React Native. In order to solve this problem, let's create a file called rn-cli.config.js on the root folder:

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

Next, write the following line at the beginning of the file index.js

```
require('node-libs-react-native/globals');
```

Now we are going to add the source code to App.js, that is the main source component in this project:

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

When you are adding the code to the App.js file, remember to replace the information for the variables clientId, clientSecret and accessToken.

Finally, let’s create an "image" folder and place the Vimeo icon file.

<img style="width: 100px" src="https://jeanpierobom.github.io/assets/images/vimeo.png" alt="Vimeo Logo">

You can download this image file [here](https://jeanpierobom.github.io/assets/images/vimeo.png).

#### Final Product

This images illustrates how to app look like after finishing this tutorial.

<img style="width: 50%" src="https://jeanpierobom.github.io/assets/images/project-a-tutorial-image-5.PNG" alt="Final Product">

In order to make this tutorial as simple as possible, when the user clicks on a video thumbnail, the video is opened on the default web browser. On improvement could be to add the Vimeo player for the video to be opened directly in the app.
 
#### Resources

Tutorial: React Native YouTube Replica<br>
Author: Marlon Decosta<br>
[https://medium.com/react-native-training/react-native-youtube-replica-f378200d91f0](https://medium.com/react-native-training/react-native-youtube-replica-f378200d91f0)
