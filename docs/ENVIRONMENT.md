# Environment variables

Environment variables read by the Delivr CLI sources under `script/`, where they are used, and behavior when unset.

| Variable | Required | Files | Usage |
|----------|----------|-------|--------|
| `HOME` | Yes (for config & iOS debug) | `command-executor.ts`, `commands/debug.ts` | See below |
| `LOCAL_APP_DATA` | No (Windows override) | `command-executor.ts` | See below |
| `CODE_PUSH_NODE_ARGS` | No | `command-executor.ts`, `react-native-utils.ts` | See below |

---

## `HOME`

**Defined in:** `script/command-executor.ts`, `script/commands/debug.ts`

### CLI config file path

The CLI stores login and session data in `.code-push.config` in the user profile directory:

```59:59:script/command-executor.ts
const configFilePath: string = path.join(process.env.LOCAL_APP_DATA || process.env.HOME, ".code-push.config");
```

- On Unix and macOS, `HOME` is the usual location (for example `/Users/you`).
- If both `LOCAL_APP_DATA` and `HOME` were missing, path resolution would be invalid; normal shells set `HOME`.

### iOS simulator system log (`debug ios`)

For `code-push-standalone debug ios` on macOS only, the CLI tails the booted simulator log:

```91:92:script/commands/debug.ts
    const logFilePath: string = path.join(process.env.HOME, "Library/Logs/CoreSimulator", simulatorID, "system.log");
    return childProcess.spawn("tail", ["-f", logFilePath]);
```

If `HOME` is wrong or unset, log streaming may fail or read the wrong path.

---

## `LOCAL_APP_DATA`

**Defined in:** `script/command-executor.ts`

Same expression as the config path above: `LOCAL_APP_DATA` is preferred over `HOME` when both apply.

- **Windows:** Often `C:\Users\<user>\AppData\Local`. Use this so `.code-push.config` lives under the expected local app data folder.
- **Unix / macOS:** Usually unset; `HOME` is used.

---

## `CODE_PUSH_NODE_ARGS`

**Defined in:** `script/command-executor.ts`, `script/react-native-utils.ts`

Optional string of extra arguments, split on whitespace and **prepended** to spawned command argument lists for React Native bundling and Hermes.

### React Native bundle (`runReactNativeBundleCommand`)

```1522:1527:script/command-executor.ts
  const reactNativeBundleArgs: string[] = [];
  const envNodeArgs: string = process.env.CODE_PUSH_NODE_ARGS;

  if (typeof envNodeArgs !== "undefined") {
    Array.prototype.push.apply(reactNativeBundleArgs, envNodeArgs.trim().split(/\s+/));
  }
```

Those tokens appear before the React Native CLI entry and the `bundle` subcommand.

### Hermes bytecode (`runHermesEmitBinaryCommand`)

```21:26:script/react-native-utils.ts
  const hermesArgs: string[] = [];
  const envNodeArgs: string = process.env.CODE_PUSH_NODE_ARGS;

  if (typeof envNodeArgs !== "undefined") {
    Array.prototype.push.apply(hermesArgs, envNodeArgs.trim().split(/\s+/));
  }
```

The same variable prepends tokens to the Hermes binary arguments (before `-emit-binary`, etc.). Despite the name, in this path they are Hermes argv prefixes, not Node flags.

**Unset:** No extra arguments are added.

---

## Not read in `script/`

The CLI does not read `NPM_TOKEN`, `NODE_ENV`, or GitHub-specific variables inside `script/` for core behavior. Parent tools (`npm`, CI, shells) may still set them for their own use.

---

## Quick reference

| Goal | Set |
|------|-----|
| Config directory on Windows | `LOCAL_APP_DATA` |
| Extra tokens for RN bundle / Hermes spawns | `CODE_PUSH_NODE_ARGS` (space-separated) |
| Normal use on macOS / Linux | Rely on `HOME` (set by default in terminals) |
