# Open-Set Face ID

A browser-based, real-time face identification system. This project demonstrates open-set face recognition entirely in the browser using MediaPipe for detection and landmarks, and ONNX Runtime Web for embedding extraction. All processing and storage are local to the user's browser; no data is uploaded to external servers by the application.

Live demo: [https://somtoonyekwelu.github.io/open-set-face-ID-js/](https://somtoonyekwelu.github.io/open-set-face-ID-js/)

## Highlights

* Real-time face detection and landmarking with MediaPipe.
* Face alignment using a 5-point Umeyama alignment mapped to a canonical template.
* Embedding extraction using an ONNX face recognition model (runs in the browser with ONNX Runtime Web).
* Open-set enrollment: enroll new identities on the fly and recognize them later.
* Gallery export and import via JSON for backup or transfer.
* Privacy-first design: embeddings and labels are stored only in localStorage in the browser.

## Versioning

This repository is at version 2. Major improvements since the face-alignment fix include: 
* Robust enrollment workflow,
* Alignment preview modal,
* Prevention of race conditions during enrollment,
* Import/export of gallery JSON, and
* Improved label rendering.

## Use cases

* Attendance systems for classrooms and workplaces.
* Access control prototypes and small-scale kiosk applications.
* Security monitoring and operator alerts.
* Research, prototyping, and teaching for face analysis and biometrics.

## Quick start

1. Clone the repository:

```bash
git clone https://github.com/somtoonyekwelu/open-set-face-ID-js.git
cd open-set-face-ID-js
```

2. Serve the project with a static server. Two quick options are shown below.

Using Python 3 simple server:

```bash
python3 -m http.server 8080
```

Using the npm package `serve`:

```bash
npx serve
```

3. Open your browser to:

```
http://localhost:8080
```

4. Allow camera access when the browser prompts you. Wait for the models to load. The first load may be slower because WebAssembly and the ONNX model are downloaded and initialized.

## Recommended browser and performance tips

* For best results use recent versions of Chrome, Edge, or Firefox.
* Enable WebGL in the browser for faster ONNX execution when available.
* The first run downloads WASM and model files; subsequent runs are faster.
* If performance is limited, reduce processing frequency via the UI control "Process every N frames".

## How it works (high level)

1. MediaPipe face detector produces bounding boxes and coarse landmarks.
2. MediaPipe face mesh produces detailed landmarks for the faces.
3. The detection bounding box is used to crop the face region.
4. The 5-point landmarks (eyes, nose, mouth corners) are used to compute an Umeyama affine transform to align the face to a canonical template.
5. The aligned crop is passed into the ONNX face recognition model to produce a normalized embedding.
6. A tracker associates embeddings over time and assigns persistent IDs. The user can enroll an ID with a friendly name.

## Usage instructions

* Enroll a person:

  1. Make sure the person is visible and is the largest face on the screen.
  2. Click the "Enroll" button. A confirmation modal will open with a preview of the aligned crop and the track ID.
  3. Enter a name and click "Confirm Enroll".

* Matching/identification:

  * The application compares live embeddings to stored embeddings and assigns a label if similarity is above the threshold. Adjust similarity threshold in the UI.

* Export and import:

  * Export saves the gallery and names into a JSON file for backup.
  * Import restores a previously exported JSON; importing replaces the current local gallery.

## File structure (important items)

* `index.html` - main web application file containing the UI and logic.
* `model.onnx` - the face recognition ONNX model (not included in the repository by default for size reasons). Place your model at the indicated path.
* `README.md` - this file.

## Security and privacy

* No server-side storage by default. All enrolled embeddings and labels are stored in the browser's localStorage.
* Users should be informed that clearing browser data will remove stored galleries.
* If you plan to host a public demo with multiple users, add explicit privacy disclosures and consider server-side protections and user consent.

## Troubleshooting

* If the camera does not start, ensure your browser has camera permissions and no other application is blocking access.
* If no faces are detected, check lighting and camera framing. Also verify that MediaPipe assets are loaded without errors in the console.
* If ONNX execution falls back to a slow backend, make sure the browser supports WebGL and try enabling hardware acceleration.

## Contribution guide

Contributions are welcome. Please follow these steps:

1. Fork the repository.
2. Create a branch for your feature or fix:

```bash
git checkout -b feature/my-feature
```

3. Make small, focused commits with clear commit messages.
4. Run and test changes locally.
5. Push your branch and open a pull request describing the change and how to test it.

Suggested areas for contribution:

* Performance improvements and smaller model support.
* Better tracking strategies and re-identification across longer gaps.
* UI improvements, accessibility, and mobile responsiveness.
* Documentation improvements and example datasets.

## Licensing

This project is available under the MIT License. If you include third-party models or assets, ensure their licenses allow your intended use.

## Credits

* MediaPipe team for face detection and mesh.
* ONNX Runtime Web contributors for browser inference tools.

## Contact and live demo

Try the live demo here: [https://somtoonyekwelu.github.io/open-set-face-ID-js/](https://somtoonyekwelu.github.io/open-set-face-ID-js/)

If you have questions or want to contribute, please open an issue or pull request on GitHub.
