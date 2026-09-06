# CodeScanner

`CodeScanner` opens a native scanner modal and delivers scanned values. Call this after [Installation](/docs/readme#installation) and camera permission setup. Start from a user action such as a button. Register `addListener` before `present` so the first catch is not missed. `present` resolves when the modal closes (after a scan or when the user cancels). Remove the listener handle afterward so rescans do not stack listeners.

## present

Scan one known QR code and confirm `event.code` in the listener, then close the modal (or let a single-scan close it):

```typescript
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

For continuous multi-scan, keep the same listen → present → remove flow and set `isMulti: true`. Do not call `addListener` again without removing the previous handle:

```typescript
await CodeScanner.present({
  detectionWidth: 0.8,
  detectionHeight: 0.2,
  isMulti: true,
});
```

`isMulti: true` keeps the modal open so you can scan many codes until the user closes it. Option fields are on the [API](/docs/api#scanneroption) page.

## Filtering code types

In version 8.0.3, the published TypeScript types expose `metadataObjectTypes` while the native implementation expects `CodeTypes`. For this version, use the defaults (`qr`, `code39`, `ean13`).

The event payload is `{ code: string }`. See [API](/docs/api) for signatures.
