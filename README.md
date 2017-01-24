# GSLanguagePickerController

[![CI Status](http://img.shields.io/travis/0x5e/GSLanguagePickerController.svg?style=flat)](https://travis-ci.org/0x5e/GSLanguagePickerController)
[![Version](https://img.shields.io/cocoapods/v/GSLanguagePickerController.svg?style=flat)](http://cocoapods.org/pods/GSLanguagePickerController)
[![License](https://img.shields.io/cocoapods/l/GSLanguagePickerController.svg?style=flat)](http://cocoapods.org/pods/GSLanguagePickerController)
[![Platform](https://img.shields.io/cocoapods/p/GSLanguagePickerController.svg?style=flat)](http://cocoapods.org/pods/GSLanguagePickerController)

## Screenshoot

![](2017-01-24 16_02_14.gif)

## Features

- No additional bundle resources
- Full language supported
- Runtime language switch
- No relaunch application
- Much like system `ViewController` in `Settings > General > Language & Region > iPhone Langage`.

## Usage

Present `GSLanguagePickerController`:

```objc
- (IBAction)selectLanguageAction:(UIButton *)button {
    GSLanguagePickerController *vc = [GSLanguagePickerController new];
    UINavigationController *navi = [[UINavigationController alloc] initWithRootViewController:vc];
    [self presentViewController:navi animated:YES completion:nil];
}
```

Or push `GSLanguagePickerController`:

```objc
- (IBAction)selectLanguageAction:(UIButton *)button {
    GSLanguagePickerController *vc = [GSLanguagePickerController new];
    [self.navigationController pushViewController:vc animated:YES];
}
```

And you can also observing language changed notification `NSCurrentLocaleDidChangeNotification`:

```
- (void)viewDidLoad {
    [super viewDidLoad];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(languageChanged:) name:NSCurrentLocaleDidChangeNotification object:nil];
}

- (void)languageChanged:(NSNotification *)notification {
    NSString *languageId = notification.object;
    NSLog(@"Language has changed: %@", languageId);
    
    NSLocale *locale = [NSLocale localeWithLocaleIdentifier:[NSBundle defaultLanguage]];
    NSString *languageName = [locale displayNameForKey:NSLocaleIdentifier value:languageId];
    UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"Language has changed" message:[NSString stringWithFormat:@"%@: %@", languageId, languageName] preferredStyle:UIAlertControllerStyleAlert];
    UIAlertAction *action = [UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:nil];
    [alertController addAction:action];
    [self presentViewController:alertController animated:YES completion:nil];
    
    // TODO: reload viewcontroller or other works
}

- (void)dealloc {
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}
```

## Reference

[Language and Locale IDs - Apple Developer](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/LanguageandLocaleIDs/LanguageandLocaleIDs.html)

## Requirements

GSLanguagePickerController require iOS 8.0+.

## Installation

GSLanguagePickerController is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "GSLanguagePickerController"
```

## Known issue

The legacy localization bundle name in `UIKit.framework` has various types.

1. `[English Name].lproj`
	
	Examples: `English.lproj`, `French.lproj`, `German.lproj`, `Spanish.lproj`
	
2. `[Language Id]_[Region Id].lproj`
	
	Examples: `zh_CN.lproj`, `zh_HK.lproj`, `zh_TW.lproj`, `en_GB.lproj`
	
3. `[Language Id]-[Script Id].lproj`
	
	Examples: `zh-Hans.lproj`, `zh-Hant.lproj`, `es-ES.lproj`

At present, type 2 can't be supported well, so `UIBarButtonSystemItem` and other components who uses `UIKit` bundle resource, may be affected until application relaunch.

## Author

gaosen, 0x5e@sina.cn

## License

GSLanguagePickerController is available under the MIT license. See the LICENSE file for more info.
