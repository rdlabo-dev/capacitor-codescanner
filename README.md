# @rdlabo/capacitor-codescanner

<!-- rdlabo-docs-omit -->
[![npm version](https://badge.fury.io/js/@rdlabo%2Fcapacitor-codescanner.svg)](https://badge.fury.io/js/@rdlabo%2Fcapacitor-codescanner)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
<!-- /rdlabo-docs-omit -->

Barcode scanner for Capacitor that opens a native modal.

Unlike camera-preview based scanners, this plugin runs the camera inside a modal. You do not need to manage the camera view in your web assets. The plugin supports multiple barcode types and continuous multi-scan mode.

<!-- rdlabo-docs-omit -->
**Full documentation:** [https://docs.rdlabo.dev/projects/capacitor-codescanner](https://docs.rdlabo.dev/projects/capacitor-codescanner)
<!-- /rdlabo-docs-omit -->

**Documentation:** [Read the full documentation](https://docs.rdlabo.dev/projects/capacitor-codescanner)

## Install

```bash
npm install @rdlabo/capacitor-codescanner
npx cap sync
```

## Usage

See [CodeScanner](https://docs.rdlabo.dev/projects/capacitor-codescanner/docs/code-scanner) to present the modal and receive scanned codes.

<!-- rdlabo-docs-omit -->
Register a listener before calling `present`. The listener receives each scanned code.

```ts
import { CodeScanner } from '@rdlabo/capacitor-codescanner';

const scanQRCode = async () => {
  await CodeScanner.addListener('CodeScannerCatchEvent', (event) => {
    console.log('Scanned code:', event.code);
  });

  await CodeScanner.present({
    detectionWidth: 0.6,
    detectionHeight: 0.15,
    isMulti: false,
    CodeTypes: ['qr'],
  });
};
```

To scan multiple barcode types continuously, enable multi-scan mode:

```ts
const scanMultipleCodes = async () => {
  await CodeScanner.addListener('CodeScannerCatchEvent', (event) => {
    console.log('Scanned code:', event.code);
  });

  await CodeScanner.present({
    detectionWidth: 0.8,
    detectionHeight: 0.2,
    isMulti: true,
    CodeTypes: ['qr', 'code39', 'ean13', 'code128'],
  });
};
```

<!-- /rdlabo-docs-omit -->

## When to use

Use this plugin when you want a ready-to-use scanning modal without building a custom camera UI. It is useful for:

- Scanning QR codes or barcodes on receipts, products, or tickets.
- Collecting multiple codes in one session with `isMulti: true`.
- Avoiding camera permission and preview wiring in your web code.

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
## Prerelease channels

An open, non-draft pull request can be published to the npm `beta` dist-tag after its `Validation` and `Package Candidate` workflows pass. A repository owner or maintainer must add a comment whose entire body is:

```text
/beta
```

The request authorizes only the pull request head SHA that existed when the comment was added. The workflow revalidates the owner or maintainer permission and head SHA immediately before publishing. Any new commit requires CI to pass again and a fresh owner or maintainer `/beta` comment. Fork pull requests are supported. Pull requests that change a release-gating workflow cannot be beta-published until those workflow changes land on `main`.

Beta versions use `<base>-beta.pr<PR number>.sha<12-character SHA>`. The candidate is built in a read-only workflow without npm publishing credentials. The privileged release workflow publishes only the validated immutable package artifact with lifecycle scripts disabled. A notification failure cannot invalidate a successful npm publish.

When a pull request is merged into `main`, it is automatically published to `beta` only after the required CI and `Package Candidate` succeed for that exact merge commit. Direct pushes to `main` do not publish a candidate.

Only `npm run release` creates a release tag. Stable `vX.Y.Z` tags publish to npm `latest`; revision/prerelease tags publish to `next`. Neither `beta` nor `next` publishing changes the npm `latest` dist-tag.

## Maintainers

- [rdlabo](https://rdlabo.dev/)
<!-- /rdlabo-docs-omit -->

<!-- rdlabo-docs-omit -->
## License

This project is licensed under the [MIT License](./LICENSE).
<!-- /rdlabo-docs-omit -->
