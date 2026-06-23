# Deployment

## Apps Script

1. Open the Apps Script project connected to the Google Sheet or create a standalone Apps Script project.
2. Paste `apps-script/Code.gs`.
3. Deploy as a Web App.
4. Execute as the account that can read the Google Sheet and Drive folders.
5. Allow access according to the intended public website policy.
6. Confirm the execution account can read the QEF photo root folder:

```text
https://drive.google.com/drive/folders/16PN5Zxnap5018OxIcxP7_FBSCxUFhXqS
```

The API reads image files from `QEF_Pages.相片資料夾ID`, including nested child
folders, and returns Drive thumbnail URLs.

7. Test:

```text
<exec-url>?action=health
```

Expected response includes:

```json
{ "ok": true }
```

## Frontend

1. Paste the Apps Script `/exec` URL into `config.js` as `apiBaseUrl`.
2. Keep `apiMode: "jsonp"` unless the Apps Script deployment is confirmed to support direct JSON fetch from GitHub Pages.
3. Publish the folder through GitHub Pages.

## Local Preview

The site can be opened directly from `index.html`. Before Apps Script is deployed, it uses fallback sample data from `config.js`.
