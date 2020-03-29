# Part 4
## Building a Login System (Continued.)

We've setup our `App.js` to point to our `LoginScreen` component, however we haven't actually made our `LoginScreen` component yet! Let's do that now. 

Create a new file in your `screens/` directory called `LoginScreen.js` and paste the below code.

```
import * as React from 'react';
import { StyleSheet, Text, View, TextInput, Button } from 'react-native';

import * as SecureStore from 'expo-secure-store';

export default class LoginScreen extends React.Component {
  constructor(props) {
    super(props)

    // Initialize our login state
    this.state = {
      email: '',
      password: ''
    }

    console.log(props)
  }
  // On our button press, attempt to login
  // this could use some error handling!
  onSubmit = () => {
    const { email, password } = this.state

    fetch("http://stark.cse.buffalo.edu/hci/SocialAuth.php", {
      method: "POST",
      body: JSON.stringify({
        action: "login",
        username: email,
        password
      })
    })
    .then(response => response.json())
    .then(json => {
      console.log(`Logging in with session token: ${json.user.session_token}`)

      // enter login logic here
      SecureStore.setItemAsync('session', json.user.session_token).then(() => {
        this.props.route.params.onLoggedIn();
      })
    })
  }
  render() {
    const { email, password } = this.state

    // this could use some error handling!
    // the user will never know if the login failed.
    return (
      <View style={styles.container}>
        <Text style={styles.loginText}>Login</Text>
        <TextInput
          style={styles.input}
          onChangeText={text => this.setState({ email: text })}
          value={email}
          textContentType="emailAddress"
        />
        <TextInput
          style={styles.input}
          onChangeText={text => this.setState({ password: text })}
          value={password}
          textContentType="password"
          secureTextEntry={true}
        />
        <Button title="Submit" onPress={() => this.onSubmit()} />
      </View>
    );
  }
}

// Our stylesheet, referenced by using styles.container or styles.loginText (style.property)
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fafafa',
    padding: 30
  },
  loginText: {
    fontSize: 30,
    textAlign: "center",
    marginBottom: 30
  },
  input: {
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    marginBottom: 20
  },
  input: {
    height: 40,
    borderColor: 'gray',
    borderWidth: 1,
    marginBottom: 20
  }
});
```
> Aside from the different stylesheet we discussed last part, this should look very similar to React code you've written before!

The only differences in our component here, compared to our component in pure React, is the names of the components we use! Rather tha `<p>` or `<input>` components, we use `<Text>` and `<TextInput>` components.

By saving our code, our app should now be refreshed and load to our new LoginScreen, prompting us to login!

[Continue to Part 6](part6.html)