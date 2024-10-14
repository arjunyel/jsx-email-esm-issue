Node v20.18.0

```bash
npx email create BatmanEmail --out=./emails
```

```bash
ReferenceError: __dirname is not defined
    at command (file:///Users/test/test/email/node_modules/jsx-email/dist/esm/cli/commands/create.mjs:33:42)
```

Fix:

```patch
diff --git a/node_modules/jsx-email/dist/esm/cli/commands/create.mjs b/node_modules/jsx-email/dist/esm/cli/commands/create.mjs
index 503140a..43bf699 100644
--- a/node_modules/jsx-email/dist/esm/cli/commands/create.mjs
+++ b/node_modules/jsx-email/dist/esm/cli/commands/create.mjs
@@ -3,6 +3,10 @@ import { join, resolve } from 'path';
 import chalk from 'chalk';
 import mustache from 'mustache';
 import { parse as assert, boolean, object, optional, string } from 'valibot';
+import path from 'node:path';
+import { fileURLToPath } from 'node:url';
+const __filename = fileURLToPath(import.meta.url);
+const __dirname = path.dirname(__filename);
 const { log } = console;
 const CreateOptionsStruct = object({
     jsx: optional(boolean()),
@@ -30,7 +34,7 @@ export const command = async (argv, input) => {
     assert(CreateOptionsStruct, argv);
     const [name] = input;
     const { jsx, out } = argv;
-    const template = await readFile(join(__dirname, '../templates/email.mustache'), 'utf8');
+    const template = await readFile(join(__dirname, '../../../templates/email.mustache'), 'utf8');
     const data = {
         name,
         propsType: jsx ? '' : ': TemplateProps',
```
