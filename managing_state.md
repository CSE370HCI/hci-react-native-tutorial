# Managing State

This post is going to demonstrate several ways that we can manage state in react or react-native. We're going to create a very simple app that lets a user toggle light mode and dark mode on a very simple user profile.

First, create a react-native project. When you're asked what boilerplate code you want, choose minimal (because the less code there is the easier things are to understand). Replace the code in App.js with the following:
```
import React from 'react';
import {View, StyleSheet} from 'react-native';
import Home from './components/Home';
import Settings from './components/Settings';
import {lightTheme, darkTheme} from './styles/themes'

class App extends React.Component{

    constructor(){
      super()
      this.state = {
        theme:  themes.lightTheme
      }
    }
    
    toggleSwitch(){
      let newTheme = themes.lightTheme;
      if(this.state.theme==themes.lightTheme){
          newTheme = themes.darkTheme;
      }
      this.setState({
        theme: newTheme
      })
    }

    render(){
        return(
        <View style={styles.container}>
          <Home/>
          <View style={styles.bottom}>
          <Settings toggleSwitch={()=>this.toggleSwitch()} switchValue={this.state.theme==themes.lightTheme}/>
          </View>
        </View>
        );
    }
} 

const styles = StyleSheet.create({
    // In this case, flex 1 will direct the View the style is applied to to take up the entire container
    container: {
      flex: 1
   
    },
    bottom: {
      flex: 1,
      justifyContent: 'flex-end'
      //justifyContent: 'flex-end' tells the bottom view to place its contents at the bottom first. This is why the settings toggle-button is at the bottom here.
    }
});


const themes =StyleSheet.create({
  lightTheme: {
    fontSize: 20
  },
  darkTheme: {
    color: 'white',
    fontSize: 20,
    backgroundColor: 'black'
  }
});

export default App;
```
There is a lot going on here, so let's break it down. First of all, we have some state. ```theme``` is going to contain the theme that we want to apply to our app (light or dark). By default, it is light-themed. We also have a ```toggleSwitch()`` function. This function is going to change the theme from light to dark every time its called.

You will notice that we are rendering a home component, as well as a Settings component. Before this code actually works, we'll need to create those. Create a folder in the same directory as App.js called components (if it doesn't already exist), and create **Home.js** and **Settings.js**.  Home.js should contain the following code:

```
import * as React from 'react';
import { StyleSheet, View,  Text} from 'react-native';
class Home extends React.Component{


    
    render(){
        return(<View > 
            <Text style={styles.container}>Home</Text>
            </View>
            );
    }
}



const styles =StyleSheet.create({
    container: {
        fontSize: 40,
        textAlign: 'center'
    }
   
});


export default Home;
```

Settings.js should have this code:

```
import * as React from 'react';
import { StyleSheet, View,  Switch, Text} from 'react-native';

class Settings extends React.Component{

    
    render(){

        return(
            <View style = {styles.container}>
                <Text style={styles.text}>Dark</Text>
               <Switch
               trackColor='black'
               thumbColor='white'
               onValueChange = {this.props.toggleSwitch}
               value = {this.props.switchValue}/>
               <Text style={styles.text}>Light</Text>
            </View>
            );
    }
}



const styles =StyleSheet.create({
    container: {
       display: "flex",
       flexDirection: "row",
       justifyContent: "center",
       fontSize: 50
    },
    text: {
        fontSize: 30
    }

});

export default Settings;

```
Switch, which you'll see being rendered inside Settings.js, is a component which renders a toggle switch. It basically lets the user turn things on and off. You'll notice that you're passing in a lot of attributes that might seem foreign. Here's what they all do:

* value - This is the boolean value which corresponds to whether or not the switch is 'on' or 'off'. 
* onValueChange - This attribute accepts a function which actually handles the event where the user toggles the switch.
* thumbColor - The color of the circular thing you actually drag on the switch (aesthetics only)
* trackColor - The color of the switch's "track". (aesthetics only)

We defined value in App.js, to just be a boolean value equal to whether or not the theme in App's state is lightTheme. If it is, we pass in true and the switch is toggled to 'light'. Otherwise, it's toggled to dark. Every time the switch is toggled, 'onValueChange'

So this is cool, except we aren't actually applying our themes to anything. Let's change that by creating a file called ```Profile.js ``` inside our components folder. Place the following code inside of it:

```
import * as React from 'react';
import { StyleSheet, View,  Text} from 'react-native';

class Profile extends React.Component{
  
    
    render(){
        const theme = this.props.theme;

        return(
              <View > 
                <View>
                <Text style={theme}> Name: Daniel </Text>
                <Text style={theme}> Age: 21 </Text>
                <Text style={theme}> Favorite Color: Blue </Text>
                </View>
                }

              </View>

    
            );
    }
}
export default Profile;

```

You can replace my data with your data if you'd like. We're going to render this inside of our ```Home``` component. Inside of home, add in Profile by importing it and placing the component below home. Your home file should look like this:

```
import * as React from 'react';
import { StyleSheet, View,  Text} from 'react-native';
import Profile from './Profile'
class Home extends React.Component{
    
    render(){
        return(<View > 
            <Text style={styles.container}>Home</Text>
            <Profile/>
            </View>
            );
    }
}

const styles =StyleSheet.create({
    container: {
        fontSize: 40,
        textAlign: 'center'
    }
   
});

export default Home;

```
So, we've added in this profile, now we can apply the theme to it! I'm going to show you 4 different ways to accomplish this, each one of which is a way that you can pass data between components.

## 1. Prop Drilling

Prop drilling is the practice of passing components from a parent component down several nested components to some great-great-grandchild component. In our case, we really don't have too far down to go, but sometimes you may be passing state down through many, many levels. App passes its state as a prop down to home, and home passes it down to profile. If we do this, we can apply the theme to our profile with no problem. Let's get this done.

In App.js, pass ```this.state.theme``` into ```<Home>```, so that your render method looks like the following:

```
render(){
        return(
        <View style={styles.container}>
          <Home theme={this.state.theme}/>
          <View style={styles.bottom}>
          <Settings toggleSwitch={()=>this.toggleSwitch()} switchValue={this.state.theme==themes.lightTheme}/>
          </View>
        </View>
        );
    }
```

Next, in home, pass in the theme prop into ```Profile```, so that we have the following render method in ```Home```:

```
 render(){
        return(<View > 
                    <Text style={styles.container}>Home</Text>
                    <Profile theme={this.props.theme}/>
                </View>
              );
    }

```

Now, through our prop-drilling, we can actually apply our themes! Toggle the switch to check it out!

[Click here to see the complete prop-drilling code](https://snack.expo.io/@danielmconnolly/a9e0e1)

## 2. Context API

Prop-Drilling is cumbersome and obnoxious in large apps, because we have to worry about props getting successfully passed down through a ton of levels. To avoid doing this, React has something called [the context api](https://reactjs.org/docs/context.html). To implement this, we're going to first need to create a context. Create a new file called ```context.js```  in the same directory as ```App.js``` and paste in the following code:

```
import {createContext} from 'react';

const {Consumer, Provider} = createContext({})

export {Consumer, Provider};
```

We're going to use ```createContext``` to create a context object which will hold a value to pass to what we call "consumers" of the context. Consumer and Provider are components that we're going to use in our app to use the context value to pass data between components, replacing the need for prop-drilling.

Now, in App.js, import the ```Provider``` component from ```context.js``` Then, wrap your ```Home``` component with the ```Provider``` component. ```Provider``` accepts an attribute called ```value```, which is going to contain the value that we want to communicate to the "consumer" component. In this case, we want to communicate the theme to "Profile", so we set ```value``` to ```{this.state.theme}```. Your app render method should look like this:

```
    //App.js render method
    render(){

        return(
            
        <View style={styles.container}>
          <Provider value={this.state.theme}>
          <Home theme={this.state.theme}/>
          <View style={styles.bottom}>
          <Settings toggleSwitch={()=>this.toggleSwitch()} switchValue={this.state.theme==themes.lightTheme}/>
          </View>
         </Provider>
        </View>
        );
    }
```

Notice that home doesn't have ```this.state.theme``` passed in as a prop anymore. We're not going to need to do that with context. The last step to make this work is to go into ```Profile.js``` to have that component "consume" the value of the Provider. Go ahead and import the ```Consumer``` component from ```context.js```. You can then add the following code into the ```Profile``` render method.

```
//Profile.js render method
    render(){
        return(
              <View >
                <Consumer>
                {theme =>
                  <View>
                  <Text style={theme}> Name: Daniel </Text>
                  <Text style={theme}> Age: 21 </Text>
                  <Text style={theme}> Favorite Color: Blue </Text>
                   </View>
                }
              </Consumer>  
              </View>
            );
    }

```

Here, we have wrapped our profile info in the ```Consumer``` component. Inside the consumer tag, we define a function that takes the value the Consumer listens for, and outputs jsx using that value to render something meaningful. 

We've successfully implemented the context API!

[Check out a working version of the code here](https://snack.expo.io/@danielmconnolly/context-api)

## 3. Component Composition

Component composition is best explained by [visiting this link](https://reactjs.org/docs/composition-vs-inheritance.html). Component composition is extrememly powerful, and it make components a lot more useable. It's going to allow us to get rid of the Profile.js component all together. 

The first step is going to be to go into ```Home.js```. In the spot where we were rendering ```Profile```, we can actually render ```this.props.children```. This is going to let us render pretty much anything inside of ```Home```, making it a much more flexible, reusable component.  Your ```Home.js``` file should now look like this:

```
import * as React from 'react';
import { StyleSheet, View,  Text} from 'react-native';

class Home extends React.Component{
    
    render(){
        return(<View > 
                    <Text style={styles.container}>Home</Text>
                    {this.props.children}
                </View>
              );
    }
}

const styles =StyleSheet.create({
    container: {
        fontSize: 40,
        textAlign: 'center'
    }
   
});

export default Home;
```

So like I said, we can remove ```Profile.js```. Before you delete this file, copy the ```Text``` components with your/my personal info. We're going to be pasting them inside of App.js. Now, go ahead and delete ```Profile.js```. Don't worry, nothing's going to blow up. Head over to ```App.js```. Replace your render function with this one:

```
    render(){
        return(
        <View style={styles.container}>
          <Home>
                <Text style={this.state.theme}> Name: Daniel </Text>
                <Text style={this.state.theme}> Age: 21 </Text>
                <Text style={this.state.theme}> Favorite Color: Blue </Text>
          </Home>
          <View style={styles.bottom}>
          <Settings toggleSwitch={()=>this.toggleSwitch()} switchValue={this.state.theme==themes.lightTheme}/>
          </View>
        </View>
        );
    }
```
You'll notice I've pasted the ```Profile.js``` stuff into the Home component. This is how we use Component Composition. We're essentially passing in all these text components as ```props.children``` to Home. The only thing different is that the ```style``` attributes are now passed ```this.state.theme```. Amazingly, the theme toggle already works! We're applying the theme directly inside of App. Your app is also simpler now, which is always great news.

[Check out a working version of this code here](https://snack.expo.io/@danielmconnolly/component-composition)