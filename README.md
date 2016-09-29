# xcunsign/xcrestore

## About

`xcunsign` and `xcrestore` are two scripts that allow for easy swapping between signed and unsigned copies of Xcode.

## Why?

Xcode 8 disables the ability to run 3rd party plugins (such as [Alcatraz]), in favor of providing an official extensions API ([WWDC: Using and Extending the Xcode Source Editor](https://developer.apple.com/videos/play/wwdc2016/414/)). This is a great thing for security and preventing the next [XcodeGhost 👻](https://en.wikipedia.org/wiki/XcodeGhost), and it sounds like the Xcode engineers want to provide the extension points that the community is asking for. However, only the source editor extension is available right now, which means that some of our favorite plugins are disabled until official support becomes available.

## Security

In light of the security benefits of using a signed Xcode, I would recommend swapping back to the signed version before any deployment builds are generated. These scripts can be integrated with [fastlane](https://fastlane.tools) to ensure that all deployemnt builds are generated from the signed Xcode, while you continue to use the unsigned version for access to plugins during development.

Fastlane has an action called `verify_xcode` which can be run as part of your `Fastfile` to ensure that the Xcode being used for the build is properly signed.

## Usage

To unsign, call the script, passing in the version of Xcode that you want to unsign. The script will find the copy of Xcode in the `/Applications` directory with that version, and run [`unsign`](https://github.com/steakknife/unsign) on it. It will keep a copy of the original signed binary as `Xcode.signed` and then replace the `Xcode` binary with `Xcode.unsigned`.

NOTE: When unsigning, the signed, original binary will be stored in an `artifacts/` directory located relative to this calling script. It's important to not delete this directory, otherwise you will not later be able to re-sign. 

```
./xcunsign.sh 8.0
```
 
 To restore the signed binary, `Xcode` will be restored to the copy in `Xcode.signed`.

 ```
./xcrestore.sh 8.0
```


## Roadmap

- If there is only one version of Xcode installed, it shouldn't be necessary to pass in the version.
- Implement a fastlane plugin to xcrestore before the build 

## Credits

Special thanks to [steakknife's unsign](https://github.com/steakknife/unsign) and [mklement0's fileicon](https://github.com/mklement0/fileicon).

## License

MIT
