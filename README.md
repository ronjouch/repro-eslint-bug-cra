This is a minimal reproduction case for a regression in eslint-module-utils@2.3.0, causing invalid `Resolve error: webpack with invalid interface loaded as resolver` errors to be emitted at runtime.

As demonstrated by `npm run demo`, reverting to eslint-module-utils@2.2.0 inside `node_modules/eslint-plugin-import` fixes the issue.

Here is the full output of `npm run demo` on my machine:

```bash
11:41:16 ~/repro-eslint-bug-cra (master|✔) npm run demo

> repro-eslint-bug-cra@0.1.0 demo /home/ronanj/repro-eslint-bug-cra
> rm -rf ./node_modules && npm run --silent 1_failedstate; npm run --silent 2_workingstate; npm run --silent 3_summary

***** FAILED STATE *****
added 1896 packages in 6.629s

/home/ronanj/repro-eslint-bug-cra/src/App.js  1:1   error    Resolve error: webpack with invalid interface loaded as resolver  import/namespace  1:1   error    Resolve error: webpack with invalid interface loaded as resolver  import/no-unresolved
  1:1   error    Resolve error: webpack with invalid interface loaded as resolver  import/named
  1:1   error    Resolve error: webpack with invalid interface loaded as resolver  import/default
  1:1   warning  Resolve error: webpack with invalid interface loaded as resolver  import/no-duplicates
  1:1   warning  Resolve error: webpack with invalid interface loaded as resolver  import/no-named-as-default
  1:1   warning  Resolve error: webpack with invalid interface loaded as resolver  import/no-named-as-default-member
  1:34  error    Unable to resolve path to module 'react'                          import/no-unresolved
  2:18  error    Unable to resolve path to module './logo.svg'                     import/no-unresolved
  3:8   error    Unable to resolve path to module './App.css'                      import/no-unresolved

/home/ronanj/repro-eslint-bug-cra/src/App.test.js
  1:1   error    Resolve error: webpack with invalid interface loaded as resolver  import/namespace  1:1   error    Resolve error: webpack with invalid interface loaded as resolver  import/no-unresolved
  1:1   error    Resolve error: webpack with invalid interface loaded as resolver  import/default
  1:1   warning  Resolve error: webpack with invalid interface loaded as resolver  import/no-duplicates
  1:1   warning  Resolve error: webpack with invalid interface loaded as resolver  import/no-named-as-default
  1:1   warning  Resolve error: webpack with invalid interface loaded as resolver  import/no-named-as-default-member
  1:19  error    Unable to resolve path to module 'react'                          import/no-unresolved
  2:22  error    Unable to resolve path to module 'react-dom'                      import/no-unresolved
  3:17  error    Unable to resolve path to module './App'                          import/no-unresolved

/home/ronanj/repro-eslint-bug-cra/src/index.js
  1:1   warning  Resolve error: webpack with invalid interface loaded as resolver  import/no-named-as-default
  1:1   error    Resolve error: webpack with invalid interface loaded as resolver  import/namespace
  1:1   warning  Resolve error: webpack with invalid interface loaded as resolver  import/no-named-as-default-member
  1:1   error    Resolve error: webpack with invalid interface loaded as resolver  import/default
  1:1   warning  Resolve error: webpack with invalid interface loaded as resolver  import/no-duplicates
  1:1   error    Resolve error: webpack with invalid interface loaded as resolver  import/no-unresolved
  1:19  error    Unable to resolve path to module 'react'                          import/no-unresolved
  2:22  error    Unable to resolve path to module 'react-dom'                      import/no-unresolved
  3:8   error    Unable to resolve path to module './index.css'                    import/no-unresolved
  4:17  error    Unable to resolve path to module './App'                          import/no-unresolved
  5:32  error    Unable to resolve path to module './serviceWorker'                import/no-unresolved

✖ 30 problems (21 errors, 9 warnings)





***** WORKING STATE *****
+ eslint-module-utils@2.2.0
added 10 packages from 7 contributors and audited 13 packages in 0.785s
found 0 vulnerabilities


/home/ronanj/repro-eslint-bug-cra/src/App.js
  1:34  error  Unable to resolve path to module 'react'       import/no-unresolved
  2:18  error  Unable to resolve path to module './logo.svg'  import/no-unresolved
  3:8   error  Unable to resolve path to module './App.css'   import/no-unresolved

/home/ronanj/repro-eslint-bug-cra/src/App.test.js
  1:19  error  Unable to resolve path to module 'react'      import/no-unresolved
  2:22  error  Unable to resolve path to module 'react-dom'  import/no-unresolved
  3:17  error  Unable to resolve path to module './App'      import/no-unresolved

/home/ronanj/repro-eslint-bug-cra/src/index.js
  1:19  error  Unable to resolve path to module 'react'            import/no-unresolved
  2:22  error  Unable to resolve path to module 'react-dom'        import/no-unresolved
  3:8   error  Unable to resolve path to module './index.css'      import/no-unresolved
  4:17  error  Unable to resolve path to module './App'            import/no-unresolved
  5:32  error  Unable to resolve path to module './serviceWorker'  import/no-unresolved

✖ 11 problems (11 errors, 0 warnings)





***** SUMMARY *****

- Failed state did yield many `Resolve error: webpack with invalid interface loaded as resolver` errors

- Working state (after revert to eslint-module-utils@2.2.0) no longer has these errors

Note: there still are import/no-unresolved errors. Please ignore them, they are artifacts of my
minimal reproduction case and my Webpack-fu is too low to fix them in the Create-React-App webpack
config. The point remains: failed state with eslint-module-utils@2.3.0 has these `Resolve error`s,
and working state with eslint-module-utils@2.2.0 no longer has them
```