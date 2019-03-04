---
title: Native Navigation Components
author:
- Samuel Roth
date: 3 March, 2019
---

# RFC0000: Native Navigation Components

## Summary

The following statement comes from the React Native website:

> The apps you are building with React Native aren't mobile web apps because React Native uses the same fundamental UI building blocks as regular iOS and Android apps. Instead of using Swift, Kotlin or Java, you are putting those building blocks together using JavaScript and React.

This proposal describes a new way of working with native navigation components in React Native projects. Specifically, I suggest we maintain a suite of navigation components for iOS and Android platforms (and more, down the line) that derive their APIs *directly* from their respective platforms.

**I believe this proposal might be best-implemented as an expansion of the [proposal](https://github.com/react-native-community/discussions-and-proposals/pull/49) to separate iOS and Android components into their own libraries under a `@react-native` monorepo.**

## Basic example

``` jsx
// App.ios.js

import { View, Text, Button, StyleSheet } from '@react-native/core';
import { NavigationBar, NavigationItem, NavigationController } from '@react-native/ios';

function Home() {
    return (
        <NavigationItem title="Home">
            <View style={styles.testView}>
                <Text>Hello world!</Text>
                <Button
                    title="Go to About screen"
                    onPress={() => NavigationController.push(About)} 
                />
            </View>
        </NavigationItem>
    );
}

function About() {
    return (
        <NavigationItem title="Home">
            <View style={styles.testView}>
                <Text>This is a proposal!</Text>
            </View>
        </NavigationItem>
    );
}

function App() {
    return (
        <NavigationController root={Home}>
            <NavigationBar prefersLargeTitles />
        </NavigationController>
    );
}

const styles = StyleSheet.create({
    testView: {
        flex: 1,
        alignItems: 'center',
        justifyContent: 'center'
    }
});

export default App;
```

## Motivation

When we consider native components made available in the React Native core, navigation solutions are not given enough attention. I am not even sure if they have been officially deprecated or not.

## Detailed design

## Drawbacks

- **Increases the size and scope of the React Native core.** However, I think this is done in a meaningful way. I think 
- **Requires developers to write more iOS- and Android-specific code.** 

## Alternatives

Alternatives such as `react-navigation` and `react-native-navigation` are listed on the website as better-maintained alternatives. However:

- **Neither of these alternatives really embrace the React development experience.** Consider the introductory examples for [React Navigation](https://reactnavigation.org/docs/en/hello-react-navigation.html) and [React Native Navigation](https://wix.github.io/react-native-navigation/#/docs/top-level-api); the heavy-lifting of managing navigators is done with JavaScript objects, with each platform having a suite of configuration options that are documented across a series of websites. Neither solution is particularly intuitive for React devs making their first foray into mobile development.
- **The *native* alternative, React Native Navigation, requires substantial changes to React Native project config.** From my personal experience, these config changes (described on their [installation](https://wix.github.io/react-native-navigation/#/docs/Installing) page) have never gone smoothly. Because navigation is an important aspect of mobile development, it should have higher priority in this library.

## Adoption strategy

## How we teach this

## Unresolved questions
