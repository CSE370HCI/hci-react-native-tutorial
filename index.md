# Part 1
## Creating a new project

**React Native** is a powerful framework that lets us create cross platform mobile applications using React concepts. **Expo** is a set of tools and services built around React Native to assist in developing and deploying React Native applications onto Android, iOS, and even the web.

In this first part of the tutorial we will be using Expo to create a new application, then we will preview it on our mobile device as well as in the browser. *For your final project in CSE 370, you will not need your React Native application working in the browser, this is purely for demo purposes.*

### Lets get started

First we'll need to start by compiling our dependencies and requirements. In your command line, use `npm` to install the `expo-cli` onto your machine. This will allow you to make, control, and deploy your mobile app. You can install `expo-cli` by running the following command.

```
npm install -g expo-cli
```

Once installed, navigate to where you would like to create your app, then run the following command to initialize a new project:

```
expo init YourProjectName --npm
```
> The `--npm` flag ensures we're using the Node Package Manager to install files. By default it uses `yarn` which we are not using for this tutorial.

When prompted, use the arrow keys in your terminal to select the `tabs` template, press enter to confirm.

![Screenshot of Expo Init](/assets/expo_init_screenshot.jpg)

> This template will allow us to create an application with an existing navigation setup. It will also default to a bottom tab navigation system, however you are free to change this if you think something else would suit your app more.

Wait for the process to complete, then navigate to the new project folder by typing `cd YourProjectName` and running `npm start`. In a browser window, a control panel will open up that will give you a few options.

[Continue to Part 2 ->](part2.html)