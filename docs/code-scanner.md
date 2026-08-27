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
    CodeTypes: ['qr'],
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
    CodeTypes: ['qr', 'code39', 'ean13', 'code128'],
  });
};
```

`isMulti: true` keeps the modal open so you can scan many codes.

<!-- !::present:: -->

<!-- !::ScannerOption:: -->

<!-- !::MetadataObjectTypes:: -->

## addListener

```typescript
import { CodeScanner } from '@rdlabo/capacitor-codescanner';

const handle = await CodeScanner.addListener('CodeScannerCatchEvent', (event) => {
  console.log('Scanned code:', event.code);
});

await handle.remove();
```

<!-- !::addListener.CodeScannerCatchEvent:: -->

<!-- !::PluginListenerHandle:: -->
