# Third-Party Notices

Enjoy Git includes software developed by third parties. Enjoy Git's proprietary
license does not alter the licenses that apply to the third-party software
identified below.

## Visual Studio Code (Code - OSS)

Portions of Enjoy Git's commit graph layout and SVG graph rendering are adapted
from Visual Studio Code's Source Control history graph implementation:

- Project: Visual Studio Code (Code - OSS)
- Source file: `src/vs/workbench/contrib/scm/browser/scmHistory.ts`
- Source revision: `25c94ab342a6b167d4b97ade0829955d4f7e094e`
- Source URL: https://github.com/microsoft/vscode/blob/25c94ab342a6b167d4b97ade0829955d4f7e094e/src/vs/workbench/contrib/scm/browser/scmHistory.ts
- License: MIT

The adapted portions have been modified for Enjoy Git, including its Git data
model, dense pagination and sparse filtering, Vue-based rendering, variable row
height, graph tooltips, and merge/fan-out geometry.

Copyright (c) Microsoft Corporation. All rights reserved.

MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## dugite-native and bundled Git

Windows and macOS distributions use Git binaries prepared by
[dugite-native](https://github.com/desktop/dugite-native). Linux uses system Git.

The current client resource-preparation configuration pins:

- Release: [`v2.53.0-4`](https://github.com/desktop/dugite-native/releases/tag/v2.53.0-4).
- Binary build: `v2.53.0-4098283`, for Windows and macOS, x64 and arm64.
- License: GNU General Public License, version 2; the unmodified upstream text is
  included in [`docs/licenses/dugite-native-GPL-2.0.txt`](docs/licenses/dugite-native-GPL-2.0.txt).
- Upstream source and build scripts: [the pinned source tree](https://github.com/desktop/dugite-native/tree/v2.53.0-4).
- Git source revision: [the Git submodule at that release](https://github.com/desktop/dugite-native/tree/v2.53.0-4/git).
- Build documentation: [setup](https://github.com/desktop/dugite-native/blob/v2.53.0-4/docs/setup.md)
  and [build overview](https://github.com/desktop/dugite-native/blob/v2.53.0-4/docs/overview.md).
- Other component versions and sources: [dependencies.json](https://github.com/desktop/dugite-native/blob/v2.53.0-4/dependencies.json).

Retrieve the pinned source tree with its Git submodule using:

```sh
git clone --branch v2.53.0-4 --recurse-submodules https://github.com/desktop/dugite-native.git
```

The [application invocation example](docs/dugite-native-usage.ts) preserves the
code previously published in the Enjoy Git release repository's README. It
illustrates application integration; it is not the complete corresponding source
of the bundled Git executables. The current client has additional runtime and
credential handling.

GPL-2.0 section 3 covers distribution of executables and their complete
corresponding source, including compilation and installation scripts. Source and
license materials must match the binaries actually distributed, including any
additional bundled components; an application invocation example alone does not
fulfil those source-distribution requirements. Older Enjoy Git installers may
use a different dugite-native release than the current resource configuration.
