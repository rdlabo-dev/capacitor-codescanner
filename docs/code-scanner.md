# CodeScanner

`CodeScanner` opens a native scanner modal and delivers scanned values. Call this after [Installation](/docs/readme#installation). Register `addListener` before `present` so the first catch is not missed.

## present

```typescript
import { CodeScanner } from '@rdlabo/capacitor-codescanner';

const scanQRCode = async () => {
  await CodeScanner.addListener('CodeScannerCatchEvent', (event) => {
    console.log('Scanned code:', event.code);
  });

  await CodeScanner.present({
    detectionWidth: 0.6,
    detectionHeight: 0.15,
    isMulti: false,
  });
};

const scanMultipleCodes = async () => {
  await CodeScanner.addListener('CodeScannerCatchEvent', (event) => {
    console.log('Scanned code:', event.code);
  });

  await CodeScanner.present({
    detectionWidth: 0.8,
    detectionHeight: 0.2,
    isMulti: true,
  });
};
```

`isMulti: true` keeps the modal open so you can scan many codes. Option fields are on the [API](/docs/api#scanneroption) page.

> **Known limitation in v8.0.3:** the public TypeScript interface exposes `metadataObjectTypes`,
> but the native implementations still read the legacy `CodeTypes` key. Because the legacy key is
> not part of `ScannerOption`, omit code-type filtering with this release.

## addListener

```typescript
import { CodeScanner } from '@rdlabo/capacitor-codescanner';

const handle = await CodeScanner.addListener('CodeScannerCatchEvent', (event) => {
  console.log('Scanned code:', event.code);
});

await handle.remove();
```

The payload is `{ code: string }`. Signatures are on the [API](/docs/api) page.
