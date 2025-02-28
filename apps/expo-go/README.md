# Bloom Go installation guide

1. Recursively clonse the repo:  

```sh
git clone —recurse-submodules https://github.com/wpmed92/expo.git
```
2. Install direnv (https://direnv.net) before you cd into the dir (make sure to add it to your shell source file)  
3. `cd expo` (you will see that direnv activates env vars as you cd into expo)
4. `git checkout bloom-go` -> branched off of `sdk-52`, since expo-go is broken on main
5. `sudo yarn` -> sudo, just to make sure we don’t bump into weird permission issues
6. `sudo yarn setup:native` -> info: react-native is a submodule in the expo repo, it’s under react-native-lab
7. `cd packages/expo`
8. `sudo yarn build` -> runs compilation in watch mode, can exit once you see there are no errors
9. `cd apps/expo-go/ios`
10. `bundle install`
11. `pod install` -> installs all the dependencies of the iOS app
12. Open `apps/expo-go/iOS/Exponent.xcworkspace` in XCode
13. On `“Exponent -> Targets -> Expo Go -> Signing & Capabilities”` tab select a provisioning profile, can be a personal Apple ID
14. On this same page change the bundle identifier to something other than `“host.exp.Exponent”` (add 12345 at the end or some other change)
15. Remove the `ExpoNotificationServiceExtension` from `Targets`
16. Remove anything that’s not supported with personal accounts (only Background Modes should remain), or if you have a team, you can leave those in
17. For each other targets select a provisioning profile
18. `sudo yarn start` in `apps/expo-go` -> starts `Metro` on port `:80`, has to keep running during the XCode build because it uses it for a build phase
19. Build and run the iOS app -> the default `EXBuildConstants.plist` uses a `PUBLISHED` `DEV_KERNEL_SOURCE`, so at first run it won’t be your `LOCAL` kernel
20. After you built and run, in both `ios/Client/Exponent/Supporting/EXBuildConstants.plist` and `/EXBuildConstants.plist.json` change `DEV_KERNEL_SOURCE` to `LOCAL`, and `IS_DEV_KERNEL` to `true`
21. Rerun the app -> should be your local app, you should see `“Bloom Development Servers”` instead of the normal “Development Servers” header
