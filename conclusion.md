# Conclusion
## Some final notes:

This tutorial hopefully got you started! You'll be able to create a nice functional basic app from what you've seen here. Remember, if you have questions please do ask on Piazza.


### Navigating to a new page
Whenever we'd like to navigate to a new page we can use the navigator prop. All direct children of a Screen (`Stack.Screen` in `App.js`) will have a `navigator` property passed to it.

For instance, if you'd like to navigate to the "Links" page from the "HomePage", you can use the following code.

```
this.props.navigator.navigate("Links")
```

The name for the destination corresponds with the `Screen` name. In this case, see the `BottomTabNavigator.js` file.

### Storing, Getting, and Deleting data on the phone
For the login session storing we use `SessionStorage` in this tutorial. Here's a quick overview of how you can set, get, and delete data there.

**Set Data**
```
SecureStore.setItemAsync('some-key').then(() => {
  console.log("This code runs whenever the data has finished being stored);
})
```

**Get Data**
```
SecureStore.getItemAsync('some-key').then(returnValue => {
  console.log("I got something " + returnValue);
});
```

**Delete Data**
```
SecureStore.deleteDataAsync('some-key').then(() => {
  console.log("This code runs whenever the data has finished being deleted);
})
```

Full documentation of SecureStore can be found on the Expo website [https://docs.expo.io/versions/latest/sdk/securestore/](https://docs.expo.io/versions/latest/sdk/securestore/)