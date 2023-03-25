This is a precommit hook that applies a patched version of prettier that
recognizes the `py-script`, `py-config` and `py-repl` tags and does not reformat their
contents. The patch that has been applied to prettier is as follows:

```patch
--- a/src/language-html/utils/index.js
+++ b/src/language-html/utils/index.js
@@ -106,7 +106,8 @@ function isTextLikeNode(node) {
 function isScriptLikeTag(node) {
   return (
     node.type === "element" &&
-    (node.fullName === "script" ||
+    (node.fullName === "py-script" ||
+    (node.fullName === "py-repl" ||
+      node.fullName === "py-config" ||
+      node.fullName === "script" ||
       node.fullName === "style" ||
       node.fullName === "svg:style" ||
       (isUnknownNamespace(node) &&
-- 
```

This patched version of prettier is hosted at https://github.com/hoodmane/prettier/tree/pyscript.
To update prettier to a new version:
1. clone `hoodmane/prettier` and checkout the `pyscript-3.0.0-alpha.6` branch
2. `git fetch upstream --tags`
3. `git rebase HEAD~1 --onto <NEW_TAG>` where for instance `NEW_TAG` might be `3.0.0-alpha.7`.
4. Force push this to `hoodmane/prettier`
5. `yarn build` (you may need to run `npm i -g yarn` to install yarn first).
6. In this repo, `rm -rf dist`
7. Copy the `prettier/dist` directory into this repo.
8. Commit the new version and tag it.
