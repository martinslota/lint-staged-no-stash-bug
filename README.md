# Minimal reproduce for bug in `lint-staged` with `--no-stash` flag

## Steps to reproduce

1. Clone this repository and install dependencies using `npm ci`

1. Create a new file and stage the changes:

   ```sh
   $ echo 'function test() {}' > index.js
   
   $ git add index.js      
   ```

1. Make another change to the file with staged changes:

   ```sh
   $ echo 'function test2() {}' >> index.js
   ```

1. Commit the changes:

   ```sh
   $ git commit -m "Adding test() without test2()"
   ⚠ Skipping backup because `--no-stash` was used.
   
   ⚠ Skipping hiding unstaged changes from partially staged files because `--no-stash` was used.
   
   ✔ Preparing lint-staged...
   ✔ Running tasks for staged files...
   ✔ Applying modifications from tasks...
   [main 4a8ad81] Adding test() without test2()
    1 file changed, 2 insertions(+)
    create mode 100644 index.js
   ```

## Expected

The staged change was committed while the unstaged change should remain as an uncommitted change.

## Actual

Both the staged and unstaged changes got committed:

```diff
$ git diff HEAD^                               
diff --git a/index.js b/index.js
new file mode 100644
index 0000000..f6b75cd
--- /dev/null
+++ b/index.js
@@ -0,0 +1,2 @@
+function test() {}
+function test2() {}
```
