# week0-2

기본적인 이론 내용은 스파르타 코딩 클럽 TypeScript 문법 종합반에 잘 나와있어서 <br>
해당 내용을 토대로, 학습한 내용을 시나리오 별로 간단하게 정리합니다.

## 시나리오 1
- TypeScript 소스코드를 작성하여 컴파일하고 JavaScript 파일로 변환하여 실행하는 경우
- src/index.ts 소스 코드를 구현합니다.
- tsconfig.json을 아래의 설정에 맞춥니다.

```json
{
  "compilerOptions": {
    "rootDir": "./src",
    "outDir": "./dist"
  }
}
```

<br>

이때 rootDir을 특정 경로 즉, ./src로 설정하게 되면, 해당 디렉터리를 타입스크립트 입력 파일의 기준으로 잡는다는 뜻이 되며, 아래와 같은 경우 에러가 발생합니다.
- ./src가 아닌 루트 디렉터리 하위의 모든 디렉터리 안 ts 파일이 있는 경우
- ./src가 아닌 루트 디렉터리 하위의 모든 디렉터리 안 js 파일이 있는 경우 (allowJs: true의 경우에만)

즉, ./src를 타입스크립트 입력 파일의 기준으로 잡으니 다른 디렉터리에는 ts 파일을 두지 말라는 것입니다.

<br>

하지만 명시적으로 선언해 주는 경우에는 에러가 발생하지 않습니다. <br>
컴파일이 되는 경로를 직접 명시적으로 지정하려면 include를 사용하면 됩니다. 이 경우, src/ 디렉터리 밖의 ts 파일은 전혀 신경 쓰지 않습니다. <br>
(exclude도 존재하며 include랑 같은 레벨 json에서 같은 형식으로 사용하면 됩니다. 이때 include보다 높은 우선순위를 가집니다.)

```json
{
  "include": [
    "src/**/*.ts"
  ],
  "compilerOptions": {
    "target": "es2016",
    "rootDir": "./src",
    "outDir": "./dist",
    "noImplicitAny": true
  }
}

```

<br>

그다음, tsc를 이용하여 컴파일(build)를 진행합니다.

```shell
tsc --build
```

<br>

그다음, 아래의 명령어를 사용하여 컴파일된 파일을 실행합니다.
```shell
node ./dist/index.js
```

<br>

## 시나리오 2
- 기존의 레거시 JavaScript 코드를 TypeScript 코드로 변환하고 싶은 경우
- 시나리오 1의 설정을 토대로 진행하겠습니다.

<br>

루트 디렉터리에 test.js 파일을 생성합니다.

```javascript
/**
 * @param {number} a
 * @param {number} b
 * @returns {number}
 */
export function add(a, b){
  return a + b;
}
```

<br>

아래처럼 설정해 줍니다.

```json
{
  "include": [
    "test.js"
  ],
  "compilerOptions": {
    "declaration": true,
    "emitDeclarationOnly": true,
    "allowJs": true,
    "checkJs": true,
    "target": "es2016",
    "outDir": "./types",
    "noImplicitAny": true,
    "esModuleInterop": true
  }
}
```

<br>

```shell
tsc --build
```

.js 파일이 .d.ts 파일로 변환됩니다.