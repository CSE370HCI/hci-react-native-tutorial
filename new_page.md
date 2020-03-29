# Creating A New Page

Creating a new page isn't a daunting task! We just need to do **three** things!

1. We need to add a new component for our page.
2. We need to connect that page to our Router
3. We need to navigate to that page from somewhere!

Let's start by creating a new component in our `screens/` directory called `ExampleScreen.js`.

In it, we can paste some sample code
```
import * as React from 'react';
import { StyleSheet, Text } from 'react-native';
import { ScrollView } from 'react-native-gesture-handler';

export default function ExampleScreen() {
  return (
    <ScrollView style={styles.container} contentContainerStyle={styles.contentContainer}>
      <Text>Hi there!</Text>
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fafafa',
  },
  contentContainer: {
    paddingTop: 15,
  }
});
```

This will initialize us with a basic Hello World page. A `ScrollView` lets content scroll if it's larger than the height of the screen, and a `<Text>Hi</Text>` element will just show basic text on the page.

### Connecting it to our Router
Next we'll want to add a tab for this in our navigator so we can visit it! Open up the `BottomTabNavigator.js` file in your `navigation/` directory. We should see two `<BottomTab.Screen>` components, let's add another one.

First, import the component we created above with the following code
```
import ExampleScreen from '../screens/LinksScreen';
```

Before the closing `</BottomTab.Navigator>` tag, add the following code.

```
<BottomTab.Screen
  name="ExampleScreen"
  component={ExampleScreen}
  options={{
    title: 'Example Title',
    tabBarIcon: ({ focused }) => <TabBarIcon focused={focused} name="md-trophy" />,
  }}
/>
```
> You can find a list of icons available at [https://expo.github.io/vector-icons/](https://expo.github.io/vector-icons/)

The `name` prop is used to identify the route. If we ever want to programatically navigate to this page, we can use the `name`.

The `component` prop is used to identify the component that should be displayed when the screen is active.

The `options` prop defines the title and icon we're using. `TabBarIcon` is a helper component that selects an icon from the Ionicon pack.