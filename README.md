[![npm version](https://badge.fury.io/js/ionic-mocks.svg)](https://badge.fury.io/js/ionic-mocks)
[![Build Status](https://travis-ci.org/stonelasley/ionic-mocks.svg?branch=master)](https://travis-ci.org/stonelasley/ionic-mocks)
[![Coverage Status](https://coveralls.io/repos/github/stonelasley/ionic-mocks/badge.svg?branch=refactor)](https://coveralls.io/github/stonelasley/ionic-mocks?branch=refactor)
[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)


# ionic-mocks
Simple test doubles for Ionic 2+ using Jasmine Spy Objects

This project is still very early in development and there are several things I'm sorting out. However since this is only meant
to be used in your tests it should be safe to pull into a project.

## Supported Types
- ActionSheet
- ActionSheetController
- Alert
- AlertController
- App
- Content
- Config
- Events
- Form
- Haptic
- InfiniteScroll
- ItemSliding
- KeyBoard - exported as IonKeyboard
- Loading
- LoadingController
- Menu
- MenuController
- Modal
- ModalController
- Platform
- Popover
- PopoverController
- NavController
- NavParams
- StatusBar
- Storage
- Tab
- Tabs
- Toast
- ToastController
- ViewController

## Native Plugins
- 3DTouch
- File
- GoogleAnalyics
- Keyboard
- Network
- NFC
- Splashscreen
- StatusBar
- Vibration

## Installation
```bash
npm install --save-dev ionic-mocks
```

### Examples

```typescript
import {Events, AlertController} from 'ionic-angular';
import {EventsMock, AlertControllerMock} from 'ionic-mocks';

describe('MyClass', () => {

    let events: Events;
    let alertCtrl: AlertController;

    let classUnderTest: MyClass;

    beforeEach(() => {

        events = EventsMock.instance();
        alertCtrl = AlertControllerMock.instance();

        classUnderTest = new MyClass(events, alertCtrl);
    });


    it('should subscribe to events', () => {
    	expect(events.subscribe).toHaveBeenCalled();
    });

    it('should call alert create', () => {

        classUnderTest.showAlert();

        expect(alertCtrl.create).toHaveBeenCalled();
    });
});
```

### Pre-mocked return types
```typescript
import {Events, AlertController, Alert} from 'ionic-angular';
import {EventsMock, AlertControllerMock, AlertMock} from 'ionic-mocks';

describe('MyComponent', () => {

    let alert: Alert;
    let events: Events;
    let alertCtrl: AlertController;

    let classUnderTest: MyClass;

    beforeEach(() => {

        events = EventsMock.instance();
        alert = AlertMock.instance():
        alertCtrl = AlertControllerMock.instance(alert);

        classUnderTest = new MyClass(events, alertCtrl);
    });


    it('should call present on alert', (done) => {

        classUnderTest.showAlert().then(() => {
            expect(alert.present).toHaveBeenCalled();
            done();
        });

    });
});
```

### Override ionic providers in TestingModule
```typescript
TestBed.configureTestingModule({
    imports: [IonicModule],
    declarations: [MyComponentUnderTest],
    providers: [
            {provide: ViewController, useFactory: () => ViewControllerMock.instance()}
    ]
});
```
# Contributing
This project has a long way to go and is full of opportunities to contribute.
I am back in school and working so for the rest of the year expect my responsiveness on this project to be slow. If anyone is up to helping vet PRs please message me. sclarklasley@gmail.com

## Contributors
  - [Felix Whittmann](https://github.com/hfwittmann)
  - [Leif Wells](https://github.com/leifwells)
  - [Jesus Botella](https://github.com/jesusbotella)
  - [Damir Arh](https://github.com/damirarh)
  - [Rvdleun](https://github.com/rvdleun)
  - [Andrey Zakharov](https://github.com/enstain)
  - [Peter Svehla](https://github.com/psvehla)

### Frequently Asked Questions:

#### Q: I am receiving a `TypeError: jit__object_Object_33 is not a function` error. What does that mean?

Answer: This means you've done something wrong. Take a look at this example:

```ts
// DO NOT DO THIS
// USING useClass INSTEAD OF useFactory IS INCORRECT
{ provide: App, useClass: AppMock }
```
```ts
// DO NOT DO THIS
// USING useFactory WITHOUT THE FAT ARROW SYNTAX IS INCORRECT
{ provide: App, useFactory: AppMock.instance() }
```

Make sure you are using the `useFactory` property name and using the fat arrow function as the value:

```ts
// DO THIS
{ provide: App, useFactory: () => AppMock.instance() }
```
