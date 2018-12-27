# TS-Merger
Generic TypeScript Merger

[![build status](https://travis-ci.org/oasp/ts-merger.svg?branch=master)](https://travis-ci.org/oasp/ts-merger)
[![build status](https://travis-ci.org/oasp/ts-merger.svg?branch=develop)](https://travis-ci.org/oasp/ts-merger)

## Usage

```javascript
let tsm = require('@oasp/ts-merger');
let mergedCode: string = tsm.merge(baseContents, pathContents, patchOverrides);
```

Being:
- baseContents: contents of the base in a string
- patchContents: contents of the patch in a string
- patchOverrides(**true**/**false**): Being false, base will have priority in case of conflicts. With true, patch will have priority.  

### Example
```javascript
let tsm = require('@oasp/ts-merger');
let mergedCode: string = tsm.merge(baseContents, pathContents, patchOverrides);
```

## Features

The merger allows merging of this node kinds:

- ImportDeclaration
- ClassDeclaration
- Decorator
- FunctionDeclaration
- MethodDeclaration
- Parameter
- BodyMethod
- PropertyAssignment
- PropertyDeclaration
- Constructor
- ArrayLiteralExpression
- ObjectLiteralExpression
- CallExpression
- VariableAssignment

This version allows merging of TypeScript files that follow this structure:

- Array of imports
- Array of functions
- Array of variables
- Array of classes

##Examples

**Base file**
```javascript
import a from 'b';
import f from 'g';

class Example1 {
    private propertyFromBase: string;

    oneMethod() {
        let variable = 'base';
    }
}
```

**Patch file**

```javascript
import c from 'd';
import h from 'g';

class Example1 {
    private propertyFromPatch: number;

    oneMethod() {
        let variable = 'patch';
    }

    anotherMethod(){}
}

class AnotherClass {}
```
**Resultant merged code (patchOverrides=false)**

```javascript
import a from 'b';
import c from 'd';
import { f, h } from 'g';

class Example1 {
    private propertyFromBase: string;
    private propertyFromPatch: number;

    oneMethod() {
        let variable = 'base';
    }

    anotherMethod(){}
}

class AnotherClass {}
```

**Resulting merged code (patchOverrides=true)**

```javascript
import a from 'b';
import c from 'd';
import { f, h } from 'g';

class Example1 {
    private propertyFromBase: string;
    private propertyFromPatch: number;

    oneMethod() {
        let variable = 'patch';
    }

    anotherMethod(){}
}

class AnotherClass {}
```

## Future version

Next releases will include merge support for:

- InterfaceDeclaration