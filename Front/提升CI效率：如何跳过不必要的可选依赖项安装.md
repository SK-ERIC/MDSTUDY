### 提升 CI 效率：如何跳过不必要的可选依赖项安装

在使用 `fabric.js` 等依赖项时，你可能会遇到其 `package.json` 中定义了 `optionalDependencies`（可选依赖项）。这些可选依赖项在某些情况下是必要的，但在你的特定环境（如浏览器）中可能不需要，甚至会导致安装时间增加和不必要的资源浪费。本文将介绍如何在 CI 环境中跳过这些可选依赖项的安装，以提高效率。

#### 什么是可选依赖项？

在 `package.json` 文件中，可以定义 `optionalDependencies`，这些依赖项在安装过程中如果遇到错误不会导致安装失败。它们对于某些功能是可选的，但在特定环境中（如浏览器端）可能不需要。

```json
{
  "optionalDependencies": {
    "canvas": "^2.11.2",
    "jsdom": "^20.0.1"
  }
}
```

#### 为什么要跳过可选依赖项？

在我们的项目中，我们使用 `fabric.js` 进行图形处理。然而，我们发现 `fabric.js` 的 `optionalDependencies` 包含了一些我们在浏览器端不需要的依赖项，比如 `canvas` 和 `jsdom`。这些依赖项主要用于 Node.js 环境，在浏览器端并没有用处。

在 CI 环境中，安装这些可选依赖项会导致以下问题：

- **安装时间增加**：我们发现 CI 构建时间明显延长，特别是安装 `canvas` 这样的依赖项时，编译时间很长。
- **资源浪费**：不需要的依赖项占用了磁盘空间和网络带宽，导致资源浪费。
- **构建失败**：某些情况下，这些依赖项的安装失败会导致整个构建过程失败，影响开发效率。

为了避免这些问题，我们需要在 CI 环境中跳过这些可选依赖项的安装。

#### 如何在 CI 环境中跳过可选依赖项的安装？

以下是几种在 CI 环境中跳过可选依赖项安装的方法：

##### 方法一：使用 `--no-optional` 标志

在 CI 环境中运行 `pnpm install` 时，使用 `--no-optional` 标志来跳过安装可选依赖项。

```bash
pnpm install --no-optional
```

##### 方法二：设置环境变量

设置 `npm_config_optional` 环境变量为 `false`，以跳过安装可选依赖项。

```bash
export npm_config_optional=false
pnpm install
```

##### 方法三：使用 `.npmrc` 文件

在项目的根目录下创建或编辑 `.npmrc` 文件，添加以下配置以禁用可选依赖项的安装。

```ini
optional=false
```

##### 方法四：在 CI 配置文件中设置环境变量

在 CI 配置文件中设置环境变量。例如，在 GitHub Actions 中，可以这样设置：

```yaml
name: Node.js CI

on:
  push:
    branches: ["main"]
  pull_request:
    branches: ["main"]

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16.x]

    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v2
        with:
          node-version: ${{ matrix.node-version }}
      - name: Set environment variable to skip optional dependencies
        run: echo "npm_config_optional=false" >> $GITHUB_ENV
      - name: Install dependencies
        run: pnpm install --no-optional
      - name: Build project
        run: pnpm build
```

##### 方法五：使用 `pnpmfile.js` 自定义依赖项安装

在项目的根目录下创建 `pnpmfile.js` 文件，并在其中自定义安装行为，以跳过特定的可选依赖项。

```javascript
module.exports = {
  hooks: {
    readPackage(pkg) {
      if (pkg.name === "fabric") {
        delete pkg.optionalDependencies["canvas"];
        delete pkg.optionalDependencies["jsdom"];
      }
      return pkg;
    },
  },
};
```

然后运行 `pnpm install` 时，它将根据 `pnpmfile.js` 中的配置跳过特定的可选依赖项。

#### 结论

在 CI 环境中跳过可选依赖项的安装可以显著提高构建效率，节省资源，并避免不必要的依赖项冲突。根据你的具体需求，可以选择上述方法中的一种或几种来实现这一目标。希望本文对你在项目中优化依赖项管理有所帮助。

---

通过这篇文章，你可以清晰地了解到在 CI 环境中如何跳过 `fabric.js` 等库的可选依赖项安装，以及背后的原理和多种实现方法。
