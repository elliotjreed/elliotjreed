# Find unused Javascript packages

With any Node or web project you can sometimes end up with unused packages and dependencies, and it's often a lengthy procedure to find out what's not being used.

Fortunately there's an excellent application called [`depcheck`](https://www.npmjs.com/package/depcheck) to find these.

To install `depcheck` globally, run:

```bash
npm install -g depcheck
```

Then go to you project directory and run:

```bash
depcheck
```

Use the results as a guide, often your dev dependencies will be listed even though they're used.
