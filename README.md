# OsteoMap

OsteoMap is a web app for recording skeletal inventories. You organise records by site, context and specimen, then mark each bone, or each zone of a bone, as present, absent or fragmentary on an interactive skeleton diagram. Records can be exported as CSV and PDF, and backed up and restored as CSV files.

OsteoMap runs entirely in the web browser on phones, tablets and computers. It can be installed like an app and works offline once loaded. There is no server and no account: records stay on the device they were made on.

OsteoMap was developed for the Centre for Forensic Anthropology at The University of Western Australia.

**Try it online at <https://donaks1.github.io/osteomap/>.** This copy may change or be taken down without notice. To rely on OsteoMap long term, [host your own copy](#deploying-your-own-copy).

**Important:** OsteoMap is free to use and is provided **"as is"**, without warranty of any kind, and without any commitment to ongoing support, maintenance, bug fixes or updates. Records are stored **only in your browser, on your device**: they can be lost if browser data is cleared, and you are responsible for keeping your own backups. Read the [disclaimer](DISCLAIMER.md) before you rely on OsteoMap.

## Using OsteoMap

The [User Guide](docs/USER_GUIDE.md) explains how to create specimens, record findings, and import and export records. Inside the app, **Help** explains how to install OsteoMap on your device.

**Back up your records.** OsteoMap does not back up automatically and keeps no copy of your records anywhere else. Regularly use **Import / Export › Export Full Backup (.csv)** and store the file somewhere other than your device. [Data storage and backups](DISCLAIMER.md#data-storage-and-backups) explains when records can be lost and how to restore them.

## Deploying your own copy

Anyone can host their own copy of OsteoMap, for free, on GitHub Pages or any other static web host. No server, database or API keys are needed. [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md) gives step-by-step instructions, including the logo icons you need to replace first.

## Developing OsteoMap

The app is in the [`osteomap/`](osteomap) folder. It is built with React, TypeScript and Vite, and needs Node.js 24.

```bash
cd osteomap
npm ci
npm run dev       # development server, for working on the code
npm test          # unit tests
npm run test:e2e  # end-to-end tests (first run: npx playwright install chromium webkit)
npm run build     # production build in osteomap/dist: see docs/DEPLOYMENT.md
```

## Licence

OsteoMap is in the public domain. Its authors have given up all their copyright and related rights in it, as far as the law allows, under [CC0 1.0 Universal](LICENSE). Nobody needs permission to use it, and there are no conditions: anyone may use, copy, modify, host, share or sell OsteoMap, for any purpose. Where the law doesn't allow a work to be placed in the public domain, CC0 gives everyone the same freedoms as an unconditional licence.

Two things are not covered by this dedication:

- **Names and logos.** The names and logos of The University of Western Australia and the Centre for Forensic Anthropology, including the logo in the app icons, belong to them. Replace the icons before you deploy your own copy, and don't use the names or logos to suggest that they endorse your copy. Naming them to say where OsteoMap came from is fine.
- **Third-party packages.** The app includes open-source packages under their own licences, which mostly ask for their notices to be kept. A production build lists them, with their licence texts, in `third-party-licenses.txt`, which is served with the app.

## Acknowledging OsteoMap

You don't have to credit anyone. But if you use or adapt OsteoMap, or publish work that used it, please acknowledge it as:

> OsteoMap, developed for the Centre for Forensic Anthropology, The University of Western Australia.

## Credits

The idea for OsteoMap came from the Centre for Forensic Anthropology at The University of Western Australia. The Centre wrote its specifications, supplied the zonation method its bone zones follow, and guided the project throughout.

OsteoMap was designed and built in 2026 by a team of students at The University of Western Australia, as part of the unit CITS3200 Professional Computing:

- James Caporn
- Shakeel Jaumally
- Andoni Karafilakis
- Thisen Malalasena
- Tinashe Nemacha
- Yupei Zhou

## Support

OsteoMap is not actively maintained, and no technical support is provided. Issues and pull requests may not be answered. If you need changes, you are welcome to make them in your own copy.

## Repository contents

| Path | Contents |
|------|----------|
| [`osteomap/`](osteomap) | The app's source code, tests and build configuration |
| [`docs/`](docs) | The [User Guide](docs/USER_GUIDE.md) and the [deployment guide](docs/DEPLOYMENT.md) |
| [`.github/workflows/`](.github/workflows) | Automated testing, and publishing to GitHub Pages |
| [`DISCLAIMER.md`](DISCLAIMER.md) | Disclaimer, and the limits of storing records in the browser |
| [`LICENSE`](LICENSE) | The CC0 1.0 public-domain dedication |
