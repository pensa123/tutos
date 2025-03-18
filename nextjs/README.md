# next js 


For Next.js, Node.js version "^18.18.0 || ^19.8.0 || >= 20.0.0" is required.

to initiate a project you need to run 

```
npx create-next-app@latest
```

You can also do it manually but it would take more time 


## Main folders on next js 


- src
   - app 
   - pages
- public

| Folder  | Usage                         |
|---------|--------------------------------|
| `app`   | App Router                    |
| `pages` | Pages Router                  |
| `public`| Static assets to be served    |
| `src`   | Optional application source folder |

## Main files on next js 

| File                 | Usage                                      |
|----------------------|-------------------------------------------|
| `next.config.js`     | Configuration file for Next.js           |
| `package.json`       | Project dependencies and scripts         |
| `instrumentation.ts` | OpenTelemetry and Instrumentation file   |
| `middleware.ts`      | Next.js request middleware               |
| `.env`              | Environment variables                     |
| `.env.local`        | Local environment variables               |
| `.env.production`   | Production environment variables          |
| `.env.development`  | Development environment variables         |
| `.eslintrc.json`    | Configuration file for ESLint             |
| `.gitignore`        | Git files and folders to ignore           |
| `next-env.d.ts`     | TypeScript declaration file for Next.js   |
| `tsconfig.json`     | Configuration file for TypeScript         |
| `jsconfig.json`     | Configuration file for JavaScript         |


## Nested routes

|              |                 |
|--------------|-----------------|
| folder        |  Route segment |
| folder/folder | Nested route segment |

## Dynamic routes 

| | |
|-|-|
|[folder]|Dynamic route segments|
|[...folder]|Cath all route segments| 
|[[...folder]]|Optional catch all route segments| 

## Paralell and intercept routes 

| Syntax        | Usage                        |
|--------------|-----------------------------|
| `@folder`    | Named slot                   |
| `(.)folder`  | Intercept same level         |
| `(..)folder` | Intercept one level above    |
| `(..)(..)folder` | Intercept two levels above |
| `(...)folder` | Intercept from root         |



## Route groups
Route groups can be created by wrapping a folder in parenthesis: (folderName)

This indicates the folder is for organizational purposes and should not be included in the route's URL path.

