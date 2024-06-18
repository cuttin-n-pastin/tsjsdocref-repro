A minimal reproduction of a problem with JSDoc references to ambient types.

# Setup

```sh
npm install
```

# Problem

A `<reference type="...">` directive in one file effectively includes that directive in all other files. How does one make files independent?

f1.js has a reference to an ambient type definition. f2.js does not have such a reference, and should fail to compile because of an unknown type. Instead it fails to compile because a value does not satisfy the type (that it should know nothing about).

Note that tsconfig.js has `types: []` and `moduleDetection: 'force'` to try enforcing independent types.

# Run

```sh
npm run tsc
```

Current output:

```
f2.js:4:7 - error TS2322: Type '"oof"' is not assignable to type 'MyType'.

4 const f = 'oof';
        ~

Found 1 error in f2.js:4
```

Expected output:

```
f2.js:3:12 - error TS2304: Cannot find name 'MyType'.

3 /** @type {MyType} */
             ~~~~~~

Found 1 error in f2.js:3
```

The expected output can be produced when the ambient type `<reference>` directive is removed from f1.js.
