# NODE Cheatsheet
version 1

## Creating a project

```bash
mkdir project
cd project
npm init -y
```

## NPM commands

### Install production packages

```bash
npm i <package_name>
```

### Install development packages

```bash
npm i --save-dev <package_name>
```
### Uninstall a package

```bash
npm un <packlage_name>
```

### List local packages

```bash
npm list
```

### List global packages

```bash
npm list -g
```

### Update a package

```bash
npm update <package_name>
```

### Check for security issues

```bash
npm audit
```

### Fix security issues

```bash
npm audit fix
```

### Run scripts defined in package.json

```bash
npm run <script_name>
```

### Clean the NPM Cache ( Resolves Installation issues )

```bash
npm cache clean --force
```
