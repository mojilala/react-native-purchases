
# react-native-purchases

React Native Purchases is a cross platform solution for managing in-app subscriptions. A backend is also provided via [RevenueCat](https://www.revenuecat.com)

## Getting started

`$ npm install react-native-purchases --save`

### Mostly automatic installation

`$ react-native link react-native-purchases`

#### Additional iOS Setup
Purchases.framework also needs to be added to your iOS project. The npm install will download the correct framework version. 

Alternatively you can install the framework via [CocoaPods](https://cocoapods.org/pods/Purchases).

##### Create a Framework Reference in your project
1. Drag `Purchases.framework` from the `RNPurchases`sub-project under the libraries section to the outer project and create a reference. 

![](https://media.giphy.com/media/83fBXlBYPF8oxMQvhN/giphy.gif)

##### Add iOS Framework to Embedded Binaries
1. In Xcode, in project manager, select your app target.
1. Select the general tab
1. Drag `Purchases.framework` from your project to the Embedded Binaries section

![](https://media.giphy.com/media/dCCyG7rmjIyByLS9ju/giphy.gif)

Add `$(PROJECT_DIR)/../node_modules/react-native-purchases/ios` to Framework Search paths in build settings

![](https://media.giphy.com/media/1pAbuARm4TLfZKdfx3/giphy.gif)

##### Add Strip Frameworks Phase
The App Store, in it's infinite wisdom, still rejects fat frameworks, so we need to strip our framework before it is deployed. To do this, add the following script phase to your build.
1. In Xcode, in project manager, select your app target.
2. Open the `Build Phases` tab
3. Add a new `Run Script`, name it `Strip Frameworks`
4. Add the following command `"${PROJECT_DIR}/../node_modules/react-native-purchases/ios/strip-frameworks.sh"` (quotes included)

![](https://media.giphy.com/media/39zTmnsW1CIrJNk5AM/giphy.gif)

## Usage
```javascript
import Purchases from 'react-native-purchases';

Purchases.addPurchaseListener((productIdentifier, purchaserInfo, error) => {
  if (error && !error.userCancelled) {
    this.setState({error: error.message});
    return;
  }

  handlePurchaserInfo(purchaserInfo);
});

Purchases.addPurchaserInfoUpdateListener((purchaserInfo, error) => {
  if (purchaserInfo) {
   handlePurchaserInfo(purchaserInfo);
  }
});

Purchases.addRestoreTransactionsListener((purchaserInfo, error) => {
  if (purchaserInfo) {
   handlePurchaserInfo(purchaserInfo);
  }
});


let purchases = await Purchases.setup("revenuecat_api_key", "app_user_id");

let entitlements = await Purchases.getEntitlements();
this.setState({entitlements});

// later make a purchase
Purchases.makePurchase(entitlements.pro.monthly.identifier);

```
  
