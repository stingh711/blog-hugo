+++
title = "How to remove vscode's decorator warning?"
date = 2018-06-19
tags = ["vscode", "editor", "javascript"]
type = "post"
draft = false
+++

1.  Create a tsconfig.json under root directory.
2.  Add following code to tsconfig.json

```json
   {
       "compilerOptions": {
           "experimentalDecorators": true,
           "allowJs": true
       }
   }
```

1.  Restart vscode