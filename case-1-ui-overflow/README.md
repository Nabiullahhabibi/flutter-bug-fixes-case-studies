# Case 1: UI Overflow on Small Screens

## 🔴 Problem
On small screen devices, the app displayed a `RenderFlex overflowed by pixels` error when showing a `Row` containing long text and an icon.
This issue was visible during UI testing on narrow screen widths.

## 🔍 Root Cause
The `Row` widget did not constrain the width of the `Text` widget.  
Because the text was long and not wrapped inside a flexible widget, it tried to take unlimited horizontal space, causing an overflow error.

## 🛠 Fix
The `Text` widget was wrapped with `Expanded` to properly constrain its width.  
Additionally, `maxLines` and `TextOverflow.ellipsis` were applied to prevent layout breaking on small screens.

## Problematic Code

```dart
Row(
  children: [
    Text(
      "This is a very very very very very very long text",
    ),
    Icon(Icons.arrow_forward),
  ],
)
```

### Fixed Code Snippet
```dart
Row(
  children: [
    Expanded(
      child: Text(
        "This is a very very very very very very long text",
        maxLines: 1,
        overflow: TextOverflow.ellipsis,
      ),
    ),
    Icon(Icons.arrow_forward),
  ],
)
