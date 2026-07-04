# Testing

## Test Project

Tests live under `src/WinCraft.Tests/`, target `net45`, and use NUnitLite as a
WPF-enabled console EXE so STA-dependent and pure-logic tests coexist in one
harness.

When a test needs access to an internal member, make it `internal`, never
`public`, and rely on the `InternalsVisibleTo` already wired to `WinCraft.Tests`
(in `WinCraft\Properties\AssemblyInfo.Shared.cs`, guarded by `#if DEBUG`).

```powershell
dotnet build -f net45 src/WinCraft.Tests/WinCraft.Tests.csproj
src/bin/Debug/net45/WinCraft.Tests.exe
```

## Test Categories

| Category | Key technique |
|---|---|
| Pure logic | No privileges, no STA, no Window. |
| Windows integration | Requires Windows but not admin (pipes, HKCU, COM). |
| STA / WPF | `[Apartment(ApartmentState.STA)]`; create a hidden `Window` with `WindowInteropHelper.EnsureHandle()` when an HWND is needed. |
| Administrator-gated | `[Explicit]` + `Assert.Ignore` guard in `[OneTimeSetUp]`. |

## What NOT to test

- Code too simple to fail: trivial types, one-liners, framework-guaranteed
  behaviour, and cases already covered by existing tests.  Don't add a test just
  because "there should be one."  Before adding a test, ask: can this code produce
  a wrong result that existing tests wouldn't catch?
- Visible desktop, mouse simulation, blocking WPF drag-drop loops — test the
  underlying COM layer directly instead.
- Specific service states (TrustedInstaller).
- Network I/O — flaky; prefer hand-testing or a separate harness.
- **Don't complicate production code to enable a test.**  Interfaces, overloads,
  or settable static flags added solely for testing are forbidden.

## When to test

Add or update a test when the code contains non-trivial logic: parsing, mapping,
state machines, validation, algorithms, structured data transformation, or
public/internal APIs with defined behaviour.  Include it in the same commit.

## TFM coverage

Run tests on `net45`.  Build with `-f net30` only when adding TFM-specific code.
