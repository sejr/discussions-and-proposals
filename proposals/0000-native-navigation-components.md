---
title: Native Navigation Components
author:
- Samuel Roth
date: 3 March, 2019
---

# RFC0000: Native Navigation Components

## Summary

This proposal describes a new way of working with native navigation components in React Native projects. Specifically, I suggest maintaining a suite of navigation components for iOS and Android platforms (and more, down the line) that derive their APIs *directly* from their respective platforms.

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
        <NavigationController
            root={ <Home /> }
            navigationBar={
                <NavigationBar prefersLargeTitles />
            }
        />
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

The following statement comes from the React Native website:

> The apps you are building with React Native aren't mobile web apps because React Native uses the same fundamental UI building blocks as regular iOS and Android apps. Instead of using Swift, Kotlin or Java, you are putting those building blocks together using JavaScript and React.

When we consider native components made available in the React Native core, navigation solutions are not given enough attention. I am not even sure if they have been officially deprecated or not.

## Detailed design

### iOS Components

#### Stack Navigation

[**NavigationController**](https://developer.apple.com/documentation/uikit/uinavigationcontroller)

``` jsx
import { NavigationController, NavigationItem } from '@react-native/ios';
import { View, Text, Button } from '@react-native/core';

function Example() {
    return (
        <NavigationItem title="Example">
            <View style={styles.container}>
                <Button onPress={() => NavigationController.push(Example)} title="Push" />
                <Button onPress={() => NavigationController.pop()} title="Pop" />
                <Button onPress={() => NavigationController.popToRoot()} title="Pop to Root" />
            </View>
        </NavigationItem>
    );
}
```

``` tsx
interface NavigationControllerProps {

    // This is the first view rendered by the NavigationController. Any
    // component that is utilized by the NavigationController should be encapsulated
    // by the `NavigationItem` HOC to provide information to the NavigationController.
    root?: JSX.Element;
    
    // You can provide a custom navigation bar to be used across every item that is
    // presented with this NavigationController instance. This could be overridden
    // inside individual NavigationItems.
    navigationBar?: JSX.Element;
    
    // The toolbar is a small element that is displayed at the bottom of the
    // NavigationController. For example, think of the bar at the bottom of Safari on
    // iOS that has buttons to view tabs, history, etc.
    toolbar?: JSX.Element;
}
```

[**NavigationBar**](https://developer.apple.com/documentation/uikit/uinavigationbar)

``` tsx
interface NavigationBarProps {
    
    // Pretty self-explanatory. If the passed value is true, then any NavigationItem
    // that uses this NavigationBar will be presented with large titles.
    prefersLargeTitles: boolean;
    
    // This allows you to override the appearance of the title.
    titleStyle?: StyleSheet.Styles;
    
    // This allows you to override the appearance of the large title. This is required
    // because the regular title and the large title are actually two distinct views.
    largeTitleStyle?: StyleSheet.Styles;
}
```

**NavigationItem**

#### Tab Navigation

- **TabBarController**
- **TabBar**
- **TabBarItem**

### Android Components

See Unresolved Questions.

## Drawbacks

- **Increases the size and scope of the React Native core.** However, I think this is done in a meaningful way. It makes even more sense if we split platform-specific code into their own libraries within a monorepo.
- **Requires developers to write more iOS- and Android-specific code.** One of the great benefits of React Native is being able to deploy on multiple platforms with a single code base. However, with this, we are required to maintain separate files for each platform. However, one of the critiques of React Native applications is that they rarely feel truly native. This is typically because many libraries (e.g. react-navigation) use JavaScript exclusively to build their UI components.
- **Will require substantial effort to build and maintain.** 

## Alternatives

Alternatives such as `react-navigation` and `react-native-navigation` are listed on the website as better-maintained alternatives. However:

- **Neither of these alternatives really embrace the React development experience.** Consider the introductory examples for [React Navigation](https://reactnavigation.org/docs/en/hello-react-navigation.html) and [React Native Navigation](https://wix.github.io/react-native-navigation/#/docs/top-level-api); the heavy-lifting of managing navigators is done with JavaScript objects, with each platform having a suite of configuration options that are documented across a series of websites. Neither solution is particularly intuitive for React devs making their first foray into mobile development.
- **The *native* alternative, React Native Navigation, requires substantial changes to React Native project config.** From my personal experience, these config changes (described on their [installation](https://wix.github.io/react-native-navigation/#/docs/Installing) page) have never gone smoothly. Because navigation is an important aspect of mobile development, it should have higher priority in this library.

## Adoption strategy

## How we teach this



## Unresolved questions

- **Implementation details for Android.** I have not done Android development in a few years and frankly, don't remember how navigation is handled on that platform. It would be great if someone with more Android development experience could contribute ideas for an Android API design.
