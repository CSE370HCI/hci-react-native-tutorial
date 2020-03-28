# Part 4
## Building a Login System

What is the first thing a user sees when opening a social media app for the first time? A login page! Let's build one!

We're going to use a library called `SecureStore` to handle storing data in our application. This is a library we'll need to install, so **head back to your command line and type the following in your project directory:**

```
npm install install expo-secure-store --save
```

Once installed, we can now use the library! It has a very easy to understand interface and will allow us *to encrypt and securely store key-value pairs locally on the device.*

Now, head into your `App.js`. There's a lot of boilerplate code in here that frankly we won't be using, replace the contents of `App.js` with the below.

```
import * as React from 'react';
import { Platform, StatusBar, StyleSheet, View } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

import BottomTabNavigator from './navigation/BottomTabNavigator';
import LoginScreen from './screens/LoginScreen';

import * as SecureStore from 'expo-secure-store';

const Stack = createStackNavigator();

export default class App extends React.Component {
  constructor() {
    super()

    this.state = {
      session: null
    }

    // uncomment this if you'd like to require a login every time the app is started
    // SecureStore.deleteItemAsync('session')
  }
  componentDidMount() {
    // Check if there's a session when the app loads
    this.checkIfLoggedIn();
  }
  checkIfLoggedIn = () => {
    // See if there's a session data stored on the phone and set whatever is there to the state
    SecureStore.getItemAsync('session').then(sessionToken => {
      this.setState({
        session: sessionToken
      })
    });
  }
  render() {
    // get our session variable from the state
    const { session } = this.state

    return (
      <View style={styles.container}>
        {Platform.OS === 'ios' && <StatusBar barStyle="default" />}
        <NavigationContainer>
          <Stack.Navigator>
            {/* Check to see if we have a session, if so continue, if not login */}
            {session ? (
              <Stack.Screen name="Root" component={BottomTabNavigator} />
            ) : (
                <Stack.Screen
                  name="Login"
                  component={LoginScreen}
                  initialParams={
                    {
                      onLoggedIn: () => this.checkIfLoggedIn()
                    }
                  }
                />
              )}
          </Stack.Navigator>
        </NavigationContainer>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
  },
});

```

There's a lot of code here, but it's fairly simple once we break it down. The first chunk of code is just importing different components and tools to help us with our app and navigation. When we get to the line starting with `const Stack =`, this is creating a new stack based navigation system.

After, we start making a normal React component. We're going to be storing our `session` in our state if it exists. When our component mounts, we'll use `SecureStore` to determine if there's a saved session token. If there is, we'll set it in our state, and otherwise we'll leave it as null/undefined.

Next we get to our `render()` method. Here we are creating a view, initializing a NavigationContainer, and we made a change from the original code. Rather than just one "Root" Screen. We now have a "Login" screen pointing to our component `LoginScreen`. This screen will be shown whenever `session` is false (`null` and `undefined` are falsey values).

Finally we see at the very bottom something **new** and **interesting**. We're creating a stylesheet and setting our styles in our JavaScript? WHAT? While there's many ways to handle styling in React Native, this is one of them. Our stylesheet is an object of different objects. Here we have one object called `container` with `flex: 1` and a background color of `#fff`. 

We reference this stylesheet by using our `styles` variable then accessing the appropriate style value, for instance `styles.container`.

There's a lot to take in here! We'll cover more in the next session

[Part 5, Login Continued ->](part5.html)