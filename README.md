# @rdlabo/capacitor-codescanner

<!-- rdlabo-docs-omit -->
[![npm version](https://badge.fury.io/js/@rdlabo%2Fcapacitor-codescanner.svg)](https://badge.fury.io/js/@rdlabo%2Fcapacitor-codescanner)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
<!-- /rdlabo-docs-omit -->

Scan QR codes and barcodes with a native Capacitor modal.

The camera runs inside the modal, so you do not manage a camera view in your web assets. Use a single scan or continuous multi-scan; each catch delivers `event.code`.

<!-- rdlabo-docs-omit -->
**Full documentation:** [https://docs.rdlabo.dev/projects/capacitor-codescanner](https://docs.rdlabo.dev/projects/capacitor-codescanner)
<!-- /rdlabo-docs-omit -->

**Documentation:** [Read the full documentation](https://docs.rdlabo.dev/projects/capacitor-codescanner)

## Install

```bash
npm install @rdlabo/capacitor-codescanner
npx cap sync
```

### Camera permission (required before first scan)

The plugin uses the device camera. On iOS, add a usage description to your app `Info.plist` (for example `ios/App/App/Info.plist`):

```xml
<key>NSCameraUsageDescription</key>
<string>This app needs camera access to scan QR codes and barcodes.</string>
```

Android declares `android.permission.CAMERA` in the plugin manifest; the OS may still prompt at runtime when you present the scanner. After editing native config, sync and rebuild the native app (`npx cap sync`, then open Xcode / Android Studio or your usual Capacitor native build).

## Usage

See [CodeScanner](./docs/code-scanner.md). Start the scan from a user action such as a button, after install and camera setup.

<!-- rdlabo-docs-omit -->
Register a listener, present the modal from a button handler, then remove the handle after `present` settles (including when the user closes the modal without a scan):

```ts
import { CodeScanner } from '@rdlabo/capacitor-codescanner';
import type { PluginListenerHandle } from '@capacitor/core';

const scanQRCode = async () => {
  let handle: PluginListenerHandle | undefined;
  try {
    handle = await CodeScanner.addListener('CodeScannerCatchEvent', (event) => {
      console.log('Scanned code:', event.code);
    });

    await CodeScanner.present({
      detectionWidth: 0.6,
      detectionHeight: 0.15,
      isMulti: false,
    });
  } finally {
    await handle?.remove();
  }
};
```

<!-- /rdlabo-docs-omit -->

## When to use

Use this plugin when you want a ready-to-use scanning modal without building a custom camera UI. It is useful for:

- Scanning QR codes or barcodes on receipts, products, or tickets.
- Collecting multiple codes in one session with `isMulti: true`.

## Features

- **Automatic light control**: turns on the flashlight in dark environments by default.
- **Vibration feedback**: vibrates when a code is detected.
- **Detection area overlay**: shows a red frame around the active scan area.
- **Detected code highlight**: draws a red frame around the detected code.
- **Close button**: a default close button in the upper right corner.
- **Multi-scan mode**: keeps scanning until the user closes the modal when `isMulti: true`.

## Platform notes

- **iOS and Android**: fully supported.
- **Web**: not supported because the plugin requires native camera access.

## API

<docgen-index>

* [`present(...)`](#present)
* [`addListener('CodeScannerCatchEvent', ...)`](#addlistenercodescannercatchevent-)
* [Interfaces](#interfaces)
* [Type Aliases](#type-aliases)

</docgen-index>

<docgen-api>
<!--Update the source file JSDoc comments and rerun docgen to update the docs below-->

### present(...)

```typescript
present(scannerOption: ScannerOption) => Promise<void>
```

| Param               | Type                                                    |
| ------------------- | ------------------------------------------------------- |
| **`scannerOption`** | <code><a href="#scanneroption">ScannerOption</a></code> |

--------------------


### addListener('CodeScannerCatchEvent', ...)

```typescript
addListener(eventName: 'CodeScannerCatchEvent', listenerFunc: (event: { code: string; }) => void) => Promise<PluginListenerHandle>
```

| Param              | Type                                               |
| ------------------ | -------------------------------------------------- |
| **`eventName`**    | <code>'CodeScannerCatchEvent'</code>               |
| **`listenerFunc`** | <code>(event: { code: string; }) =&gt; void</code> |

**Returns:** <code>Promise&lt;<a href="#pluginlistenerhandle">PluginListenerHandle</a>&gt;</code>

--------------------


### Interfaces


#### ScannerOption

| Prop                    | Type                               | Description                                                                                                                     |
| ----------------------- | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **`detectionWidth`**    | <code>number</code>                | Width of the detection area relative to the available width (0–1). Default is 0.4.                                              |
| **`detectionHeight`**   | <code>number</code>                | Height of the detection area relative to the detection width. Default is 1 on iOS; 0.15–0.2 is typical on Android.              |
| **`enableCloseButton`** | <code>boolean</code>               | Enable close button on the top left of the scanning area (default: true)                                                        |
| **`sheetScreenRatio`**  | <code>number</code>                | Specify the ratio of the scanning area (sheet modal size) to the screen size. Default is 0.9 for android, 1(pageSheet) for iOS. |
| **`CodeTypes`**         | <code>MetadataObjectTypes[]</code> | Specify the types of codes to recognize (default: ["qr", "code39", "ean13"])                                                    |
| **`isMulti`**           | <code>boolean</code>               | Enable multi scan mode (default: false)                                                                                         |
| **`enableAutoLight`**   | <code>boolean</code>               | Enable auto light when environment is dark (default: true)                                                                      |


#### PluginListenerHandle

| Prop         | Type                                      |
| ------------ | ----------------------------------------- |
| **`remove`** | <code>() =&gt; Promise&lt;void&gt;</code> |


### Type Aliases


#### MetadataObjectTypes

<code>'aztec' | 'code128' | 'code39' | 'code39Mod43' | 'code93' | 'dataMatrix' | 'ean13' | 'ean8' | 'face' | 'interleaved2of5' | 'itf14' | 'pdf417' | 'qr' | 'upce' | 'catBody' | 'dogBody' | 'humanBody' | 'salientObject'</code>

</docgen-api>

<!-- rdlabo-docs-omit -->
## Maintainers

- [rdlabo](https://rdlabo.dev/)
<!-- /rdlabo-docs-omit -->

<!-- rdlabo-docs-omit -->
## License

This project is licensed under the [MIT License](./LICENSE).
<!-- /rdlabo-docs-omit -->
