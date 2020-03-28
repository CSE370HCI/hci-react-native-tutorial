# Part 3
## What are all these files?

Before we modify any code, lets figure out what all our code does first! Open up your project directory and lets go through each directory and each file to figure out what it does.

**The project's file structure at a glance:**

I skipped over some less important files and directories. For the most part, you can ignore them.
```
.
├── App.js
├── assets
│   ├── fonts
│   │   └── SpaceMono-Regular.ttf
│   └── images
│       ├── icon.png
│       ├── robot-dev.png
│       ├── robot-prod.png
│       └── splash.png
├── components
│   ├── StyledText.js
│   ├── TabBarIcon.js
│   └── __tests__
│       ├── __snapshots__
│       │   └── StyledText-test.js.snap
│       └── StyledText-test.js
├── constants
│   ├── Colors.js
│   └── Layout.js
├── navigation
│   ├── BottomTabNavigator.js
│   └── useLinking.js
├── package.json
└── screens
    ├── HomeScreen.js
    └── LinksScreen.js
```

### Lets separate this up to make more sense of it.

**App.js**
Similar to our regular React project, this is the root of our application. Inside this contains all our other components.

If we open up `App.js` you'll notice toward the bottom the following React code.

```
<View style={styles.container}>
  {Platform.OS === 'ios' && <StatusBar barStyle="default" />}
  <NavigationContainer ref={containerRef} initialState={initialNavigationState}>
    <Stack.Navigator>
      <Stack.Screen name="Root" component={BottomTabNavigator} />
    </Stack.Navigator>
  </NavigationContainer>
</View>

```

Finally! Something we recognize! **In React Native, there's a lot less components than there are when we were writing websites**. There are no `div` components, nor are there `p` components. The most basic component in React Native, is a `<View>`.

The above code does a bit of checking to determine whether the user is on iOS or not to optionally display a StatusBar, something that's handled by default in Android. It then continues on to something interesting, a `<NavigationContainer>` followed by a `<Stack.Container>` followed by a `<Stack.Screen>`! What do these do?

Navigation in React Native is one of the tricky things to figure out, but the concepts aren't too difficult.

1. `<NavigationContainer>` surrounds our app that we want to be navigatable.
2. `<Stack.Navigator>` creates a new stack based navigation system. We can go back (pop off the stack), or add to the stack.
3. `<Stack.Screen>` creates a new screen, or page, for our navigation system. The demo only has one, our Root navigation. This connects to our component `BottomTabNavigation` which we will get into next.

**assets/**
This is a directory where we keep all of our assets. This usually is images, fonts, and other files.

**components/**
These are some components that aren't screens. While these can be anywhere, the were put here to keep them organized.

**constants/**
This directory contains constants that can be referenced for easy of use and updating. It might be helpful for colors, themes, and more!

**navigation/**
There are two files in the navigation directory.

- **BottomTabNavigator**: This is our main navigator that holds the different screens we can navigate to on our app. If we want to add a new tab to our bottom tab bar, we'll need to put it here!

- **useLinking.js**: We **won't** be using this for our tutorial. However if you'd like to use it, you're welcome to! This sets up our routing for easy linking so we can use URL like routes to navigate between pages.

**package.json**
Just like our React project, this is where our project info, dependencies, and scripts are stored.

**screens/**
The final directory is the most important. For organiation, we'll be putting all our new pages in this directory! Keeping an organized codebase is important, even if you're the only person working on it!

[Continue to Part 4 ->](part4.html)