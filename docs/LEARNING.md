# Trace FinForge report creation

FinForge won the Capital One Best Financial Hack sponsor prize at [HopHacks Fall 2021](https://devpost.com/software/finforge). This fork preserves the authors' project. The [original practice lab](https://github.com/ErwinHe7/hackathon-crud-lab) provides offline CRUD and exercises.

Read `client/src/pages/Report.jsx` → `routes/post.js` → `models/postMod.js`, then `client/src/pages/Library.jsx`. The form sends `PUT /api/post`; the route creates a Mongoose document and saves it.

## Fix included in this fork

The create route previously saved the report, then referenced the undefined variable `data` in its response. That returned `500` after a successful write. Retrying could duplicate the report. The route now returns the saved document with `201`; the route method and client path remain compatible.

```sh
node --test test/create-report.test.cjs
```

Two tests run the real route with a test-double model and router: one asserts a single save and a response containing the saved identity; one asserts a failed save is not reported as successful. They do not connect to MongoDB or exercise the React UI.

Learning task: explain how a failed response differs from a failed database write. Write an additional test for a delayed save before changing behavior. Then compare full CRUD in the local lab with this project's create/list/comment/vote routes; edit and delete are not present here.

The legacy app needs disposable configuration and broader integration checks before live use. Further study targets include the form's `fraudType` field naming, checkbox state, and response-status handling.
